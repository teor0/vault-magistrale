#magistrale #soa 
[[Algoritmi non bloccanti]]
# Read copy update
Read Copy Update (RCU) viene utilizzato per workload read intensive e strutture dati condivise. L'idea di base è che ci sia sempre un writer e che la concorrenza sia tra lettori e scrittori. In particolare i puntatori in uscita di strutture dati rimosse logicamente non vengono distrutti prima di essere sicuri che nessun lettore stia ancora attraversando la copia modificata della struttura dati. Il riutilizzo del buffer (e quindi il rilascio) avviene al termine di un cosiddetto “periodo di grazia”, consentendo ai lettori permanenti non linearizzati dopo l'aggiornamento di procedere comunque. Si va quindi a serializzare solo gli update. ![[rcu.png]] data l'immagine cerchiamo anche di definire i compiti dei reader e del writer. I reader o meglio <font color=orange>standing readers</font> fanno una segnalazione prima del inizio del periodo di grazia, leggono il valore dal buffer e poi segnalano che non ci sono più. Il writer prende un write lock, aggiorna la struttura dati, aspetta che gli standing readers finiscano la lettura e rilascia il buffer per il riuso, andando ad ignorare i lettori che operano sull'istanza modificata della struttura dati. Gli standing readers possono segnalare la loro presenza attraverso 2 tecniche: la prima è la non preemptable RCU, dove la read non fa perdere la CPU al thread ma è richiesto quindi di non fare operazioni bloccanti. In questo modo l'update del writer, va a deallocare su tutte le CPU il buffer e se trova un reader che non è preemptable attenderà. Si rischia però di attendere anche chi non dovrebbe usufruire del periodo di grazia così. Nella preemptable RCU invece si permettono letture bloccanti. 
Più nel dettaglio per la non preemptable RCU: il lettore deve solo disattivare la prelazione al momento della lettura e riattivarla al termine (attenzione alle interazioni con la pianificazione RT) – ma non è consentita alcuna chiamata al blocco dei servizi con possibile cambio di contesto. Chi scrive capisce che nessun lettore permanente è ancora lì grazie alla propria migrazione su tutte le CPU remote, in Linux è molto semplice effettuarlo tramite `for_each_online_cpu(cpu) run_on(cpu);` Le migrazioni creano un context switch che porta lo scrittore a comprendere che nessun lettore permanente, non linearizzato dopo lo scrittore, è ancora lì. In alternativa, le CPU pubblicano l'occorrenza del context switch e lo scrittore lo legge.
Per quanto riguarda la preemptable RCU ad esempio a user level: trovare standing readers nei periodi di grazia è un po’ più complicato. Un contatore di presenza atomico indica una lettura in corso. Lo scrittore aggiorna la struttura dei dati e reindirizza i lettori a un nuovo contatore di presenze (una nuova epoca) e si ha attesa fino al rilascio della presenza conta sul contatore dell'ultima epoca. Il contatore atomico spesso è gestito con `fetch_and_add` e può non essere singolo. Gli aggiornamenti della struttura dei dati e lo spostamento dell'epoca non sono atomici. Tuttavia, l'unico rischio che si corre è quello di aspettare qualche lettore che abbia già visto la nuova forma della struttura dati, ma si sia registrato come presente nell'ultima epoca. Vediamo una timeline possibile per preemptable RCU:
![[timeline_rcu.png|400]]
abbiamo infine delle API in Linux:
- rcu_read_lock(): segnala che il lettore ha iniziato a funzionare (ad esempio potrebbe bloccare la preemptability o non fare nulla in un kernel con CONFIG_PREEMPT = n)
- rcu_read_unlock(): questo segnala che il lettore ha terminato la sua attività
- synchronize_rcu(): questa API implementa l'attesa della fine del periodo di grazia
- call_rcu(...): questa API consente l'esecuzione dell'utilizzo di una richiamata al termine del periodo di grazia, ma non blocca il thread di aggiornamento. Qui posso passare il puntatore alla funzione di reclaim ad un thread tipicamente un demone che si occuperà della reclaim, realizzando deferred work.

e ci sono anche delle API per la sleepable RCU:
- int init_srcu_struct(struct srcu_struct \*sp);
- void cleanup_srcu_struct(struct srcu_struct \*sp)
- int srcu_read_lock(struct srcu_struct \*sp);
- void srcu_read_unlock(struct srcu_struct \*sp, int idx);
- void synchronize_srcu(struct srcu_struct \*sp);

Vediamo un'implementazione nel dettaglio di RCU, `rcu-list.c`: 
```
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>
#include <unistd.h>
#include <errno.h>
#include "list.h"

extern int main(int argc, char** argv);
extern rcu_list the_list;
void * house_keeper(void * the_list);

void rcu_list_init(rcu_list * l){[...]}
int rcu_list_search(rcu_list *l, long key){[...]}

int rcu_list_insert(rcu_list *l, long key){
	element * p;
	p = malloc(sizeof(element));
	if(!p) return 0;
	p->key = key;
	p->next = NULL;
	AUDIT
	printf("insertion: waiting for lock\n");
	pthread_spin_lock(&(l->write_lock));
	//traverse and insert
	p->next = l->head;
	asm volatile("mfence");
	l->head = p;
	asm volatile("mfence");
	AUDIT
	while(p){
		printf("elem %ld\n",p->key);
		p = p->next;
	}
	pthread_spin_unlock(&l->write_lock);
	return 1;
}

int rcu_list_remove(rcu_list *l, long key){
	element * p, *removed = NULL;
	int grace_epoch;
	unsigned long last_epoch;
	unsigned long updated_epoch;
	unsigned long grace_period_threads;
	int index;
	pthread_spin_lock(&l->write_lock);
	//traverse and delete
	p = l->head;
	if(p != NULL && p->key == key){
		removed = p;
		l->head = removed->next;
		asm volatile("mfence");//make it visible to readers
	}
	else{
		while(p != NULL){
			if ( p->next != NULL && p->next->key == key){
				removed = p->next;
				p->next = p->next->next;
				asm volatile("mfence");//make it visible to readers
				break;
			}	
			p = p->next;	
		}
	}
	//move to a new epoch - still under write lock
	updated_epoch = (l->next_epoch_index) ? MASK : 0;

    	l->next_epoch_index += 1;
	l->next_epoch_index %= 2;	

	last_epoch = __atomic_exchange_n (&(l->epoch), updated_epoch, __ATOMIC_SEQ_CST); 
	index = (last_epoch & MASK) ? 1 : 0; 
	grace_period_threads = last_epoch & (~MASK); 
	AUDIT
	printf("deletion: waiting grace-full period (target value is %ld)\n",grace_period_threads);
	while(l->standing[index] < grace_period_threads);
	l->standing[index] = 0;
	pthread_spin_unlock(&l->write_lock);
	if(removed){
		free(removed);
		return 1;
	}
	return 0;
}


void * house_keeper(void * the_list){
	int grace_epoch;
	unsigned long last_epoch;
	unsigned long updated_epoch;
	unsigned long grace_period_threads;
	int index;
	rcu_list * l = (rcu_list*) the_list;
redo:
	sleep(PERIOD);
	pthread_spin_lock(&l->write_lock);
	updated_epoch = (l->next_epoch_index) ? MASK : 0;
	//printf("next epoch index is %d - next epoch is %p\n",l->next_epoch_index,updated_epoch);

    	l->next_epoch_index += 1;
	l->next_epoch_index %= 2;	
	last_epoch = __atomic_exchange_n (&(l->epoch), updated_epoch, __ATOMIC_SEQ_CST); 
	index = (last_epoch & MASK) ? 1 : 0; 
	grace_period_threads = last_epoch & (~MASK); 
	AUDIT
	printf("house keeping: waiting grace-full period (target value is %ld)\n",grace_period_threads);
	while(l->standing[index] < grace_period_threads);
	l->standing[index] = 0;
	pthread_spin_unlock(&l->write_lock);
	goto redo;
	return NULL;
}
```
al suo interno si ha l'include del file header `list.h` che definisce le strutture per la linked list RCU. Per l'inserimento server solo il lock, niente standing o grace period. `mfence` serve a rendere effettive visibilità e raggiungibilità. Tecnicamente sono superflue dato che il rilascio del lock realizza una read modify write e quindi ha al suo interno mfence. `fetch_and_add` come detto serve a notificare l'esistenza del reader per prima istanza e poi a notificare l'uscita nel epoca in cui si è entrati. La remove è cruciale ed utilizza come detto il grace period. Il bit più significativo indica l'epoca mentre gli altri il numero di reader nel epoca per quanto riguarda la variabile `epoch`. Attenzione che tra una mfence e atomic exchange è possibile perdere la CPU. Un altro problema è che il contatore di lettori può andare in overflow. Infine il thread house keeper si occupa di cambiare l'epoca e resettare i contatori!

# Vettorizzazione
Parliamo infine della vettorizzazione, ovvero quanto un'operazione avviene su tutti gli elementi di un'array senza utilizzare un ciclo. L'istruzione di update viene fetchata una sola volta, si tratta quindi di una forma di processamento SIMD (Single Istruction Multiple Data), opposto al classico MIMD (Multiple Instruction Multiple Data) dei processori multi core o sistemi multi processore tramite speculazione OOO. SIMD si basa sull'uso di registri vettoriali e hardware con capacità di calcolo vettoria ovvero le GPU. Il supporto alla vettorizzazione in x86 avviene tramite Streaming SIM Extension (SSE), introdotto con il Pentium III, al suo interno l'op code specifica come utilizzare i registri XMM da 128 bit se come 2 entry a 64 bit double precision float, 4 entry a 32 bit single precision float e così via. ![[XMM.png]] In SSE2 si arriva ad avere un'estensione a 256 bit con i registri YMM. ![[YMM.png]] Per lavorare con i registri vettoriali XMM o YMM, le istruzioni di movimento dei dati devono operare con allineamento della memoria. `gcc` offre il supporto per allineare valori statici (array di) tramite `__attribute__((aligned (16)))`. Abilita la vettorizzazione automatica a livello di compilazione con i flag `–O` quando possibile. Chiaramente si può anche ricorrere all'allocazione dinamica della memoria con allineamento esplicito. I limiti della pagina da 4KB sono intrinsecamente allineati a 16 bit, il che aiuta con `mmap()`. L'utilizzo di istruzioni che richiedono l'allineamento su dati non allineati causerà un errore di protezione generale

---