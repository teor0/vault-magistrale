#soa #magistrale 
[[Cache coherency MSI MESI e MOESI]]
# Protocolli directory based
Sono un alternativa agli snooping dato che in quelle tipologie di protocolli non si ha scalabilità dovuto al fatto che il bus delle transazioni deve essere globalmente visibile e le comunicazioni sono broadcast. Nelle directory based invece si ha scalabilità grazie appunto alle directory (entry per ogni linea di cache) che mantengono traccia di chi gestisce le informazioni e che implementa un update punto-punto tra i processi. Vediamo lo schema di base:
![[schema_directory_based.png|400]]
si ha un sistema con $N$ processori $P_i$, in cui per ogni blocco di memoria, dove la dimensione è la singola linea cache, si mantiene una voce nella directory. Al suo interno N bit di presenza (azzurro) vengono impostati se il blocco è nella cache di $P_i$ e si ha 1 bit dirty (rosso). Vediamo qualche esempio con 3 processi:
Per primo vediamo dove $P_0$ ha un read miss nella cache:   
![[read_miss_1.png|400]]
se il bit dirty è pari a 0, si va a leggere dalla RAM, si aggiorna il bit di presenza e si salva il dato nella cache. Nel caso in cui si ha il bit dirty pari a 1:
![[read_miss_2.png|400]]
si individuano i bit di presenza, si porta il bit dirty pari a 0 rendendo la directory condivisa, si imposta il bit di presenza e si propaga il valore al lettore. 
Per quanto riguarda un processo $P_0$, che ha un write miss:
![[write_miss_1.png|400]]
se il bit dirty è pari a 0, manda valori invalidi a tutti i processi con bit di presenza pari a 1, poni pari a 0 tutti i bit di presenza dei processori, imposta a 1 il bit dirty e imposta pari a 1 il bit di presenza di $P_i$. Nel caso in cui si ha bit dirty pari a 1:
![[write_miss_2.png|400]]
ritrova i processi che hanno bit di presenza pari a 1, poni a 0 tali bit, poni pari a 1 il bit di presenza del processo 0 e mantiene dirty bit pari a 1.  

Problemi di accesso ai “campi comuni”: i campi più utilizzati all'interno di una struttura dati dovrebbero essere posizionati in testa della struttura al fine di massimizzare gli accessi alla cache. Ciò dovrebbe accadere a condizione che l'allocatore di memoria fornisca indirizzi allineati alla cache per blocchi di memoria allocati dinamicamente. I “campi vagamente correlati” dovrebbero essere posizionati sufficientemente distanti all'interno del struttura dei dati in modo da evitare penalizzazioni in termini di prestazioni dovute alla falsa condivisione della cache. Si ha il problema che viene detto <font color=red>false cache sharing</font>: ![[false_cache_sharing.png]] si hanno due strutture dati D e D' collegate alle funzioni F e F' che sono memorizzate nella stessa riga della RAM. Tale linea poi viene fatta risalire nella cache di un thread. Se F tocca D si va a prendere esclusività per modifiche e così F' deve attendere e questo impatta l'esecuzione. Esempio di codice: ![[codice_false_cache.png]]
Esistono però API di sistema o libreria per allocare bene ed evitare false caching come `posix_memalign, aligned_alloc, memalign`. 
Vediamo un esempio del false cache sharing in atto con il file `fcs.c` eseguito tramite `time taskset 0x11 a.out` dove su un unico core che utilizza una singola cache ma che anche se marcata dirty si può continuare a leggere con un altro thread la linea della cache.
```
#define SPAWNS 2 
#define CYCLES 500000000 
#ifndef TARGET 
#define TARGET 1 
#endif 
int v[128] __attribute__((aligned(64))); 
int sum; 
void* child_thread(void*p){ 
	int me = (long)p; 
	int i; 
	if (me == 0 ){ 
		printf("I'm child %d - the cache line writer\n",me); 
		for(i=0; i<CYCLES; i++){ 
			v[0]+=i; 
			asm("mfence":::); 
		} 
	} 
	else{ 
		printf("I'm child %d - the cache line reader\n",me); 
		for(i=0; i<CYCLES; i++){ 
			sum += v[TARGET]; 
			asm("mfence":::); 
		} 
	} 
} 
int main(int argc, char** argv){ 
	long i; 
	int status; 
	void *thread_status; 
	pthread_t tid; 
	for (i=0;i<SPAWNS;i++){ 
		pthread_create(&tid,NULL,child_thread,(void*)i); 
	} 
	pthread_join(tid,&thread_status); //wait for the last spawned thread 
}
```
`asm("mfence":::);` è un'istruzione machine dipendent sullo store buffer che flusha sulla cache. `TARGET` può cadere sulla stessa linea del thread che flusha la cache, ed in questo caso fa leggere dalla cache. I due cicli fanno transazioni MESI: il primo vuole essere E mentre il secondo S, quindi si mettono i bastoni fra le ruote a vicenda.

## Dettagli Flush+Reload per attacchi Meltdown/Spectre
Una tecnica (chiamata Flush+Reload) si basa sull'osservazione delle latenze di accesso ai dati condivisi. Passaggi algoritmici: 
1. il contenuto della cache relativo ad alcuni dati condivisi viene svuotato. 
2. Successivamente si accede nuovamente in modalità lettura. 
3. A seconda della tempistica di questi ultimi accessi si ricava se il dato è stato acceduto anche da qualche altro thread. 
   
L'implementazione su x86 si basa su 2 elementi costitutivi: un timer ad alta risoluzione e un'istruzione di flush della linea della cache non privilegiata. Questi passaggi algoritmici sono stati finalmente sfruttati per Meltdown/Spectre attacchi come visto. In particolare come timer ad alta risoluzione si utiliza RDTSC (Read Time-Stamp Counter) che permette di calcolare il numero di cicli di clock, mentre come istruzione di flush CLFLUSH. ![[CLFLUSH.png]] attenzione a non fare una MOV e poi chiamare CLFLUSH, dato che la MOV sposta i dati sullo store buffer! Prima di discutere nel dettaglio del funzionamento degli attacchi, introduciamo il codice che permette di eseguire Assembly in C. Istruzioni ASM inline seguono generalmente questa struttura:
```
__asm__ [volatile][goto] (AssemblerTemplate
			[ :OutputOperands]
			[ :InputOperands]
			[ :Clobbers]
			[ :GotoLabels]);
```
dove: 
- AssemblerTemplate: il codice ASM effettivo
- volatile – forza il compilatore a non intraprendere alcuna ottimizzazione (ad esempio istruzione effetto di posizionamento)
- goto: assembly può portare a passare a qualsiasi etichetta in GoToLabels
- OutputOperands: condizioni successive allo spostamento dei dati
- InputOperands: precondizioni per lo spostamento dei dati
- Clobbers: questi registri sono salvati sullo stack prima di eseguire l'assembly e poi rimessi a posto dopo l'esecuzione. EAX non è clobber.

Attenzione, il simbolo = significa che l'operando corrispondente viene utilizzato come un output. Quindi dopo l'esecuzione del blocco di codice ASM, il valore dell'operando diventa la fonte per una determinata posizione di destinazione (ad esempio per una variabile). Nel caso in cui l'operando debba mantenere un valore da utilizzare come input (quindi l'operando è la posizione di salvataggio del valore di una posizione di origine) allora non è necessario utilizzare il simbolo =. In conclusione serve solo per InputOperands. Principali specifiche degli operandi supportati da gcc:
- r: operandi di registro generici
- m: operando di memoria generico (ad esempio nello stack)
- 0-9: indice dell'operando riutilizzato
- i/I: operando immediato a 64/32 bit
- q: registro indirizzabile tramite byte (ad esempio eax, ebx, ecx, edx)
- A: eax o edx
- a, b, c, d, S, D: eax, ebx, ecx, edx, esi, edi rispettivamente (o al, rax ecc. varianti a seconda della dimensione dell'istruzione effettiva operandi)

Vediamo quindi flush reload nel dettaglio:
![[flush_reload.png|]]
![[timing_flush_reload.png|400x400]]
%1 si riferisce all'indice che etichetta il registro da utilizzare, che nel codice è ECX data la C passata come adrs come parametro di probe. Per quanto riguarda il codice l'ordine di esecuzione viene gestito tramite le istruzioni `mfence` e `lfence`. Il codice `far.c` realizza lo stesso flush+reload senza flush andando quindi a realizzare un cache miss e non un cache hit. È possibile rendere disponibile `rdtsc` solo a kernel level per evitare il calcolo del tempo?  Sì, lo stato del processore può essere configurato per eseguire questa istruzione in modalità privilegiata. Ciò richiederà il passaggio attraverso una chiamata di sistema per ottenere il valore, che potrebbe introdurre “varianza” nei campioni, portando a misurazioni meno precise, un argomento che discuteremo nuovamente. In ogni caso si può emulare un timer con un thread dello user space che si incrementa regolarmente un contatore condiviso ed avendo i due thread in core separati ma comunque "vicini". Per maggiori dettagli vedasi codice `far-emulated-timer.c` ![[timer_thread.png|400]] Ovviamente `rdtsc` rimane disponibile user level perché è utilizzato lato compilazione per calcolare il tempo, vedasi `far-gcc-intrinsics.c`. Questa API può superare in OOO quindi necessità di istruzione lfence.

Un sistema di caching inclusivo (ad esempio Intel) è tale che un componente di caching di livello inferiore Lx mantiene sempre una copia del contenuto memorizzato nella cache da qualche componente Ly di livello superiore. Quindi il contenuto Ly è sempre incluso in quello Lx. Una cache non inclusiva (ad esempio alcuni chipset AMD) non ha questo vincolo. Quindi potremmo memorizzare nella cache alcuni dati su L1 e poi, ad es. dopo aver sfrattato da L1, potremmo caricare i dati dentro, ad es. LLC. Per questi sistemi di memorizzazione nella cache, gli attacchi incrociati Flush+Reload potrebbero fallire. Possono invece essere ancora fruttuosi se utilizzati, ad esempio, tra processi in esecuzione sullo stesso core della CPU.

---