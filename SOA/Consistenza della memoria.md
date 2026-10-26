#soa #magistrale 
[[Directory based protocols e flush e reload]]

# Consistenza della memoria
Qual è il comportamento della memoria in caso di accessi simultanei ai dati? La lettura di una posizione di memoria dovrebbe restituire l'ultimo valore scritto ma l'ultimo valore scritto potrebbe non essere chiaramente o univocamente definito sotto accesso concorrente e con più posizioni come destinazione. Il modello di consistenza della memoria definisce in quale ordine le unità di elaborazione percepiscono gli accessi concorrenti, basandosi su regole di ordinamento, non necessariamente sulla tempistica degli accessi. Attenzione la consistenza della memoria non è da confondere con la coerenza della memoria! Come modelli di memoria abbiamo:
1. Program order (delle operazioni del processori): ordine degli accessi alla memoria per processore determinato dal programma software
2. Visibility order (di tutte le operazioni): ordine degli accessi alla memoria osservato da uno o più processori (ad esempio, se fossero raccolti da un osservatore esterno). Ogni lettura da una posizione restituisce il valore della scrittura più recente

L'utilizzare store buffer è chiaramente program order. Abbiamo poi il concetto di sequential consistency (consistenza sequenziale): 
> Un sistema multiprocessore è sequenzialmente consistente se il risultato di qualsiasi esecuzione è lo stesso come se le operazioni di tutti i processori fossero eseguite in un ordine sequenziale e le operazioni di ogni singolo processore apparissero in questa sequenza nell'ordine specificato dal suo programma. (Lamport 1979)

Gli accessi alla memoria basati sull'ordine del programma, non possono essere sovvertiti nella sequenza complessiva, quindi non può essere osservato che avvengano in un ordine diverso da un osservatore "remoto". Vediamo un esempio: ![[esempio_seq_cons.png|400]] Si capisce con questo esempio che utilizzare gli store buffer porta ad un'architettura non sequential consistence, inoltre la sequential consistency è “scomoda” in termini di prestazioni della memoria. Come abbiamo visto anche prima nel corso non si scrive direttamente sulla cache, ma si utilizzano store buffer e molto altro. Esempio: i fallimenti della cache devono essere serviti "in sequenza" anche se sono operazioni di scrittura senza istruzioni attualmente dipendenti. <font color=orange>Total Store Order</font> (TSO) si basa sull'idea che archiviare dati in memoria non equivale a scrivere in memoria (come avviene con il modello program order). Qualcosa si posiziona a metà tra un'operazione di scrittura (tramite software) e l'effettivo aggiornamento della memoria (nell'hardware). Una scrittura si materializza come un salvataggio in memoria quando è "più conveniente" nel tempo. Molte macchine standard si affidano a TSO (ad esempio SPARC V8, x86). Rivediamo i concetti architetturali che stanno dietro TSO che abbiamo visto precedentemente ma adesso con un po' più di precisione. 
Gli store buffer consentono di salvare scritture in memoria e/o cache per ottimizzare gli accessi di interconnessione (ad esempio quando il mezzo di interconnessione è bloccato). La CPU può continuare l'esecuzione prima che la scrittura nella cache/memoria sia completata (ovvero prima che i dati vengano salvati). Alcune scritture possono essere combinate, ad es. memoria video. L'inoltro dell'archivio consente alle letture dalla CPU locale di vedere le scritture in sospeso nello store buffer mentre rimangono invisibili alle CPU remote. ![[store_buffer.png|400]]
per quanto riguarda la timeline ![[tso_timeline.png]] su x86 le operazioni di load potrebbero essere riordinate con operazioni più vecchie di locazioni diverse e questo rompe il funzionamento del algoritmo di mutua esclusione di Dekker. 
## Operazioni di sincronizzazione e RMW
Procediamo con le operazioni di sincronizzazione della memoria per x86:
- SFENCE (store fence): esegue un'operazione di serializzazione su tutte le istruzioni store-to-memory emesse prima dell'istruzione SFENCE. Questa operazione di serializzazione garantisce che ogni istruzione di memorizzazione che precede l'istruzione SFENCE nell'ordine del programma diventi globalmente visibile prima di qualsiasi istruzione di memorizzazione che segue l'istruzione SFENCE.  
- LFENCE (Load fence): esegue un'operazione di serializzazione su tutte le istruzioni di caricamento dalla memoria emesse prima dell'istruzione LFENCE. Nello specifico, LFENCE non viene eseguito finché tutte le istruzioni precedenti non vengono completate localmente e nessuna istruzione successiva inizia l'esecuzione fino a quando LFENCE viene completato. In particolare, un'istruzione che carica dalla memoria e che precede un LFENCE riceve i dati dalla memoria prima del completamento del LFENCE.
- MFENCE (Memory fence): esegue un'operazione di serializzazione su tutte le istruzioni di caricamento dalla memoria e di memorizzazione in memoria emessi prima dell'istruzione MFENCE. Questa operazione di serializzazione lo garantisce ogni istruzione di caricamento e memorizzazione che precede l'istruzione MFENCE nell'ordine del programma diventa globalmente visibile prima di qualsiasi istruzione di caricamento o memorizzazione che segue MFENCE istruzione. MFENCE quindi combina SFENCE e LFENCE.

Le FENCE è garantito siano ordinate rispetto a qualsiasi altra istruzione di serializzazione (ad esempio CPUID, LGDT, LIDT ecc.). Alcune istruzioni possono essere precedute da LOCK e diventano istruzioni di serializzazione: ADD, ADC, AND, BTC, BTR, BTS, CMPXCHG, DEC, INC, NEG, NOT, OR, SBB, SUB, XOR, XAND. In particolare CMPXCHG è utilizzata dalle implementazioni di spinlock. Esistono poi le fence built-in di gcc come `void _mm_sfence(void)` e `bool __sync_bool_compare_and_swap(type *ptr, type oldval, type newval)`.
> Attenzione che sia SFENCE che MFENCE sono costose per l'uso di transazioni distribuite, si preferiscono quindi le istruzioni atomiche precedute da LOCK in quanto non effettuano il flush della pipeline! 

Più in generale, CMPXCHG storicamente noto come Compare-and-Swap (CAS), rientra nella classe più ampia delle istruzioni <font color=red>Read-Modify-Write</font> (RMW) come anche Fetch-and-Add, Fetch-and-Or ecc. Queste istruzioni eseguono uno schema in cui un valore viene letto e aggiornato se i criteri sono soddisfatti. Questo può essere fatto anche in modo atomico, con la garanzia di non essere interferiti da accessi alla memoria nei flussi di programmi remoti. In sostanza, il mezzo di interconnessione (ad esempio il bus di memoria) è bloccato a favore dell'unità di elaborazione che sta eseguendo l'istruzione Read-Modify-Write. Per l'architettura in cui RMW è consentito solo su dati allineati alla linea di cache, possiamo evitare di bloccare il bus di memoria e possiamo semplicemente sfruttare lo stato "esclusivo" del protocollo di coerenza della cache MESI.  Vediamo un esempio di implementazione di una barriera di sincronizzazione attiva basata su istruzioni RMW con il file `barrier-synchronization.c`:

```
long control_counter __attribute__((aligned(64))) = THREADS; //num. thread che entrano nella barriera di sincronizzazione
long era_counter __attribute__((aligned(64))) = THREADS; //fine era per m thread
//entrambi allineati a 64 per garantire il lavoro su una linea singola di cache
__thread long int me;
__thread long int cycle;

int barrier(void){
	int ret;
	while(era_counter != THREADS && control_counter == THREADS);
	ret = __sync_bool_compare_and_swap(&control_counter,THREADS,0);
	if( ret ) era_counter = 0;
	__sync_fetch_and_add(&control_counter,1); //initial update already committed when performing this
	while(control_counter != THREADS);
	__sync_fetch_and_add(&era_counter,1);
	return ret;
}
```

si ha anche la possibilità di implementare un trylock in assembly che funziona sia user che kernel level: ![[try_lock_asm.png]] a livello user può accadere che pochi thread debbano utilizzare il lock via CMPXCHG e quindi la possibilità di conflitti è bassa, mentre lato kernel questo meccanismo è poco scalabile e porta a problemi di conflitti. In particolare possiamo affermare che:
> [!important] Proprietà
> $\text{Scalabilità}\implies \text{performance}\implies \text{power usage (Watt)}$

---

