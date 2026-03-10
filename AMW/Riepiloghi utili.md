#amw #magistrale 
[[Reverse code engineering]]
# Riepilogo componenti di un calcoltore
Tra i vari componenti di un calcolatore abbiamo CPU, RAM, HDD, GPU, registri, scheda di rete, ALU, alimentatore, BUS e memoria cache. L'ultima cruciale per abbassare i tempi di esecuzione di due ordini di grandezza. Oggigiorno attraverso la DRA (direct memory access) i dispositivi accedono direttamente alla RAM attraverso il BUS. Ricordiamo anche la differenza tra architettura e organizzazione del calcolatore. La prima è quello che è visibile al programmatore mentre la seconda è tutto ciò che sta sotto a livello hardware. L'architettura su cui lavoreremo è l'architettura di Von Neumann, dove si ha una sola memoria in cui si mantiene tutto, ossia i dati e il programma stesso. In questa architettura **non esiste** una distinzione tra istruzioni macchina e dati. 
I registri della CPU possono essere a 32bit (4 byte) o a 64bit (8 byte) ed il loro contenuto è una sequenza senza un significato. Da qui tutte le rappresentazioni: unsigned, signed, complemento a 1 e complemento a 2. L'ultima come si sa è quella utilizzata che permette alla ALU di fare calcoli con numeri positivi e numeri negativi.

Tra i vari componenti del sistema operativo, come il kernel (la parte in cui si eseguisce il codice con maggiori privilegi), i tool di sistema o la shell, vi sono le librerie di sistema che sono fondamentali. In Linux la più utilizzate è *libc*. I diversi OS definiscono diverse API (application programming interface). C'è poi la user e kernel mode che non sono stati software, ma sono stati hardware! Ricordiamo che nel hardware ci sono 4 livelli di privilegio, ovvero ring level 0,1,2 e 3. Dove ring level 0 è il privilegio di kernel mode, mentre ring level 3 sono privilegi user mode.
> [!important] Definizione processo
> Il processo è un'istanza di un programma in esecuzione

^13e5e2

> [!important] Definizione programma
> Il programma è un insieme di istruzioni con un formato specifico a seconda se mantenuto sul disco rigido o sulla RAM

# Riepilogo notazione esadecimale, little endian e big endian
È la notazione che esprime da 0 a 15 utilizzando da 0 a 9 e poi a,b,c,d,e,f per 11,12,13,14,15. In C per indicare l'uso della notazione esadecimale si utilizza il prefisso 0x. Ad esempio 0x1001 dove si calcola il valore come visto più volte. La notazione è utilizzata in assembly data la corrispondenza tra esadecimale e bit. La notazione ottale è ancora usata in qualche ambito ad esempio i permessi in Linux. Ricordiamo che il valore del dato non cambia, ma solo la notazione con cui si esprime. 

Little endian e big endian ovvero l'ordine dei byte, nel primo caso si legge la sequenza dal byte meno significativo al più significativo (da destra verso sinistra), mentre nell'altro al contrario (da sinistra verso destra). L'architettura Intel è little endian mentre l'architettura ARM non è nessuno dei due ma cambia a seconda di un bit fisico tra i due ordini. Ricordiamo anche i byte in memoria hanno un indirizzo.

# Riepilogo indirizzi 
L’indirizzo fisico identifica una posizione fisica in una memoria. 
L’indirizzo generato dalla CPU mentre un programma è in esecuzione è denominato indirizzo logico. Esso è gestito dal MMU (memory management unit) che comprende l'unità di segmentazione che trasforma l'indirizzo logico in indirizzo lineare, che entrerà nell'unità di paginazione per diventare un indirizzo fisico. Oggigiorno i sistemi operativi non usano più la segmentation unit. L'indirizzo virtuale è il termine usato per i sistemi operativi moderni per l'indirizzo lineare. L'indirizzo logico è composto da due parti. Un segmentation description (o base) e un offset. La segmentation unit si occupa semplicemente di sommare la base al offset per ottenere l'indirizzo lineare. Negli OS moderni la base è pari a 0 e limite pari a $2^{32}$ per sistemi a 32 bit, mentre ci si limita a $2^{48}$ per sistemi a 64bit. Quindi anche se l'hardware fa la somma tra 0 ed un numero, nel sistema operativo l'offset è sostanzialmente l'indirizzo lineare. Tuttavia restano ancora area di memoria con segmenti che non hanno base 0 come il TLS (Thread Local Storage) che tratteremo.

I descrittori di segmento in linguaggio macchina vengono conservati in appositi registri. La grandezza dell'indirizzo fisico non è un dettaglio architetturale ma organizzativo dovuto alla RAM del calcolatore. 
Un indirizzo a 32 bit è suddiviso in tre parti: la prima di 10 bit, la seconda di 10 bit e la terza di 12 bit. La parte di 12 bit costituisce l'offset, che equivale a $2^{12}=4096$ byte che viene detto <font color=orange>page frame</font>. Il secondo blocco è detto indice di secondo livello, mentre il primo blocco è indice di primo livello. Assieme al registro *cr4*, l'indice di primo livello fornisce intervallo per trovare una voce della tabella di paginazione di primo livello e dati i 10 bit avrò $2^{10}=1024$ voci. Ciascuna voce è un puntatore ad un altra tabella di secondo livello che sommato all'indice di secondo livello, mi ritorna un altro intervallo per la voce o meglio il puntatore all'interno della tabella. Infine questo valore sommato al offset mi ritorna l'elemento all'interno della page frame. Nel caso di sistema a 64 bit, si hanno 48 bit, divisi in 5 parti, 4 da 9 bit e una da 12 bit. Aumentando il numero di indirizzazione a 4 tabelle.
![[paginazione.png]]
Ogni processo ha un proprio insieme di tabelle di paginazione, in particolare ha una tabella di primo livello differente dagli altri. Ciò significa che l'OS può costruire una mappature differente tra indirizzo lineare e fisico da processo a processo. Questo è il meccanismo alla base della memoria virtuale. I processi dunque vivono sul cosiddetto *linear address space*, che è partizionato in due per separare gli indirizzi per esecuzioni in user mode e gli indirizzi per esecuzioni in kernel mode. Le tabelle di paginazione che mappano gli indirizzi kernel mode sono le stesse (condivise) per tutti i processi, dato che tutte portano allo stesso insieme di pagine fisiche che appartiene al kernel. All'interno della sezione user mode, ci stanno il programma, lo stack che ricordiamo cresce per indirizzi decrescenti,
l'heap e le librerie dinamiche che stanno a ridosso della zona kernel mode. ![[las.png|300x400]] Questo è lo schema per sistemi a 32 bit più o meno e lo stack e l'heap possono collidere. In architetture a 64 bit, lo spazio è talmente grande che le collisioni sono estremamente improbabili.

| Parti dell'eseguibile | Nome segmento di memoria in cui è memorizzato |
| --------------------- | --------------------------------------------- |
| Codice                | text                                          |
| Dati                  | data                                          |
| costanti              | rodata                                        |
| dati inizializzati    | bss                                           |

La differenza tra dati inizializzati e dati è dato dal fatto che spesso i dati sono inizializzati pari a 0. Linux non permette di carica un eseguibile in un punto diverso dello spazio lineare, per due motivi, il primo perché è un processo lento ed oneroso, il secondo perché è inutile. Windows permette di farlo.

# Rappresentazione dei tipi
In C una semplice `int c; sizeof(c)` dipende da 3 cose: l'architettura del calcolatore, il sistema operativo e dal compilatore utilizzato. Non è quindi fissa la lunghezza. Per evitare ambiguità si devono utilizzare tipi di dati espliciti. Per Linux contenuti in stdxxx.h con xxx il tipo di dato. Mentre in Windows si usano: byte, word, dword e qword. Rispettivamente 8, 16, 32 e 64 bit.
Per le stringhe si usano varie codifiche: ascii, ascii-z o UNICODE.
Per i vettori in C la convenzione è partire da 0 fino a N-1. Ma quanto è grande un puntatore a livello di byte non è esplicitato. Ricordiamo anche che *v* in `int v[16];` è l'etichetta che si da al vettore. Per quanto riguarda un puntatore invece, `int *p`, la memoria allocata dipende solamente dall'architettura, 4 byte per 32 bit e 8 byte per 64 bit. In questo caso *p* è associato alla sequenza di celle che contengono il puntatore. Il puntatore è un indirizzo lineare che punta ad un valore. Vettori e puntatori sono identici sintatticamente ma non semanticamente. Valgono infatti `p[3]`$\equiv$ `*(p+3)` ovviamente la memoria allocata al puntatore non è detto che sia totalmente occupata, infatti sorgono i noti problemi di memoria. Di seguito la tabella dei data types in C con la loro dimensione in byte:
![[c_data_type.png]]






