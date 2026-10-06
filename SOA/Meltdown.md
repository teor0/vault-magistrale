#soa #magistrale 
# Attacco Meltdown
L'attacco Meltdown si basa sul footprint lasciato dalle operazioni phantom, in particolare: prima si flusha la cache, poi con l'intento di leggere un byte B dalla sezione kernel (operazione illegale), B verrà utilizzato come offset per leggere la memoria lato user, realizzando così operazioni phantom. In quanto le operazioni per leggere dalla memoria salgono nella cache e generano un side effect che verrà utilizzato dopo aver eliminato lo spazio d'indirizzamento. Questo perché si riproverà ad usare gli stessi dati per vedere con che velocità si è acceduto alla cache. Questo meccanismo è detto **side channel**. Se riusciamo a misurare il ritardo di accesso per gli hit e miss quando leggiamo la zona di cache interessata, si risale al valore di B. Questo attacco si basa sulla capacità del processore di eseguire speculativamente un accesso alla memoria legittima dopo un altro accesso alla memoria non legittimo. Possiamo aspettarci che più velocemente sarà servito l'accesso legittimo nel file pipeline, maggiore è la probabilità che venga completamente eseguita dopo l'accesso illegittimo si stato effettuato. Quindi, possiamo aspettarci che l'attacco possa essere eseguito con maggiore probabilità positivamente se il byte a livello di kernel è già nella cache quando proviamo a ottenerlo. Il contenuto della cache, ovvero lo stato della cache, può cambiare a seconda del design del processore, non necessariamente al commit delle istruzioni. Tali contenuti infatti non sono direttamente leggibili nell'ISA. Possiamo leggere solo dalla memoria logica, quindi il flusso di un programma idealmente non sarebbe mai influenzato dal fatto che un dato sia o meno in cache in un dato punto dell'esecuzione. L'unica cosa veramente influenzata sono le prestazioni. Prima di vedere un esempio per x86_64, diamo un'occhiata alla sua architettura che è diversa da x86![[x86_64.png]]
si nota come oltre a passare a registri appunto a 64 bit, il numero di registri general purpouse raddoppia!

Vediamo un esempio di Meltdown per processori Intel:
![[codice_meltdown.png]]
leggere 64 bit vuol dire cambiare il valore di una riga della cache che è composta da 64 byte. `jz retry` viene utilizzato perché si sta speculando, e magari il valore cambia nel frattempo e non c'è più.
Come contromisure sono state impiegate: KASLR (Kernel Address Space Randomization), limitazione della dipendenza dello spostamento massimo che applichiamo all'immagine logica del kernel. Chiaramente questo è ancora debole contro attacchi di forza bruta. KAISER (Kernel Isolation in Linux), dove si espone allo user non tutto il kernel nella page table utilizzata ma solo un parte con al suo interno binario per entry code degli interrupt. Il flush esplicito della cache ad ogni ritorno dalla modalità kernel potrebbe essere pure un'idea per ritornare nel contesto user, tuttavia oltre ad avere un impatto sulle performance non è praticabile dato che esiste l'exploit SPECTRE che può avvenire senza istruzioni illegali! Chiaramente oggigiorno i processore hanno una patch di protezione verso Meltdown.
Vediamo uno schema per KASLR
![[scema_kaslr.png]]
rip (relative istruction pointer). Da notare che prima storicamente per kernel di Linux a 32 bit, il kernel era caricato fisso dai 3GB in poi, successivamente per aumentare la sicurezza si implementerà KASLR per randomizzare l'indirizzo di partenza ad ogni boot. 
vediamo uno schema per KAISER
![[scema_kaiser.png]]
come detto per Kaiser si va ad esporre gli entry point per la gestione degli interrupt ed altro contenuto rimane isolato e sarà disponibile solo dopo un context switch. Tecnicamente su Linux si parla di PTI (Page Table Isolation) a partire dal kernel 4, in particolare si effettua il flush ogni volta che il registro che contiene la page table cambia e questo ha un costo. Tramite il parametro `pti=off` su GRUB si può disabilitare l'isolamento per avere un boost di performance a costo di minore sicurezza.
![[pti_linux.png]]
Come illustrato nella figura, si va a duplicare solo il primo livello di page table, questo comporta che le performance vengono influenzate dai due context switch necessari per operare con la page table necessaria.

# Funzionamento branch predictor
Parliamo adesso del funzionamento del branch predictor, partendo dai tipi di branch dovuto ai diversi tipi di branch.
![[SOA/img/jumps.png]]
L'execution dependency porta ad aspettare nella pipeline per effettuare il fetch delle istruzioni. 

>[!info] 
>La predizione di un salto non comporta una trap, il tutto avviene in hardware. 

Nella cache si riportano i bit meno significativi di un'istruzione per risparmiare spazio. Come supporto di base per i branch in una pipeline si ha il <font color=orange>Branch Target Buffer</font> (BTB) è una cache in cui una entry conserva l'indirizzo dell'istruzione di un branch e l'ultimo indirizzo target utilizzato come salto osservato per quel branch. Risulta estremamente utile per salti incondizionati e chiamate comuni alle subroutine. 
![[btb.png]]
Il supporto hardware per migliorare le prestazioni in pipeline (speculative) a fronte dei branch condizionali si chiama **Dynamic Predictor**. La sua effettiva implementazione consiste in un Branch-Prediction Buffer (BPB)  o Branch History Table (BHT). L'implementazione di base si basa su una cache indicizzata in base al significato dei bit meno significativi di istruzioni di salto e un bit di stato. Il bit di stato indica se il salto relativo all'istruzione di salto è stato eseguito di recente. Il flusso di esecuzione (speculativo) segue la direzione relativa alla previsione da parte del bit di stato, seguendo quindi il comportamento recente. 

>Si prevede che il passato recente sia rappresentativo del prossimo futuro!

I predittori a un bit “falliscono” nello scenario in cui il branch viene spesso preso o non preso e raramente non presi o preso. In questi scenari, portano a 2 errori successivi nella previsione, quindi 2 squash nella pipeline. Per cicli innestati questo è molto rilevante. La conclusione del ciclo interno porta a modificare la previsione, che viene comunque modificata alla successiva iterazione del ciclo esterno. I predittori a due bit richiedono 2 errori di previsione successivi per invertire la previsione. Quindi ognuno dei 4 stati stabilisce in che stato ci si trova durante l'esecuzione.
![[ex_branch_predictor.png]]
nell'esempio riportato sopra il branch predictor è invertito da ogni conclusione del ciclo interno.