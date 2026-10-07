[[Attacco Meltdown]]
# Funzionamento branch predictor
Parliamo adesso del funzionamento del branch predictor, partendo dai tipi di branch dovuto ai diversi tipi di branch.
![[SOA/img/jumps.png]]
L'execution dependency porta ad aspettare nella pipeline per effettuare il fetch delle istruzioni. 

>[!info] 
>La predizione di un salto non comporta una trap, il tutto avviene in hardware. 

Nella cache si riportano i bit meno significativi di un'istruzione per risparmiare spazio. Come supporto di base per i branch in una pipeline si ha il <font color=orange>Branch Target Buffer</font> (BTB) è una cache in cui una entry conserva l'indirizzo dell'istruzione di un branch e l'ultimo indirizzo target utilizzato come salto osservato per quel branch. Risulta estremamente utile per salti incondizionati e chiamate comuni alle subroutine. Attenzione bisogna fare prima una considerazione per la sicurezza che affronteremo meglio dopo. Al interno di un core il predittore B viene utilizzato, per gestire i salti. In questo core si sta eseguendo il thread T che utilizza salti nella zona C significa nell'address space, per il principio di località continuerà a lavorare lì in quella zona C quindi bastano i bit meno significativi per conoscere la dove saltare. Nel core viene anche eseguito il thread T' che utilizza salti nella zona C' che però condividono bit meno significativi con la zona C del thread T. L'attaccante può addestrare con il thread T il predittore per poi eseguire nell'address space di T' in modo che la predizione di quello che bisogna fare sia funzione di quanto fatto in T. 
![[btb.png]]
Il supporto hardware per migliorare le prestazioni in pipeline (speculative) a fronte dei branch condizionali si chiama **Dynamic Predictor**, dato che la condizione che si va a testare cambia dinamicamente nel tempo. La sua effettiva implementazione consiste in un Branch-Prediction Buffer (BPB)  o Branch History Table (BHT). L'implementazione di base si basa su una cache indicizzata in base al significato dei bit meno significativi di istruzioni di salto e un bit di stato. Il bit di stato indica se il salto relativo all'istruzione di salto è stato eseguito di recente. Il flusso di esecuzione (speculativo) segue la direzione relativa alla previsione da parte del bit di stato, seguendo quindi il comportamento recente. 

>Si prevede che il passato recente sia rappresentativo del prossimo futuro!

I predittori a un bit “falliscono” nello scenario in cui il branch viene spesso preso o non preso e raramente non presi o preso. In questi scenari, portano a 2 errori successivi nella previsione, quindi 2 squash nella pipeline. Per cicli innestati questo è molto rilevante. La conclusione del ciclo interno porta a modificare la previsione, che viene comunque modificata alla successiva iterazione del ciclo esterno. I predittori a due bit richiedono 2 errori di previsione successivi per invertire la previsione. Quindi ognuno dei 4 stati stabilisce in che stato ci si trova durante l'esecuzione.
![[ex_branch_predictor.png|400x300]]
nell'esempio riportato sopra il branch predictor è invertito da ogni conclusione del ciclo interno. Di seguito invece riportiamo la macchina a stati finiti del predittore a 2 bit:
![[stati_finiti_2bit.png|400x300]]
se il salto deve essere preso e si è sbagliata la previsione, il salto continua a dover essere preso. Questo permette di dimezzare il numero di errore per cicli innestati. Quindi per dover uscire devo commettere due errori e non uno come nel caso con un bit. I jump condizionali sono circa il 20% delle istruzioni in un codice. Si può gestire meglio del predittore a 2 bit? Si, l'idea è di realizzare predizione correlata, dove oltre a guardare informazioni sul passato del salto, valuto altre informazioni per correlare salti diversi. Vediamo un esempio che motiva ciò
```
if (aa==VAL)
	aa=0;
if (bb==VAL)
	bb=0;
if (aa!=bb)
	//do work 
```
qui abbiamo che l'ultimo salto è funzione delle due condizioni precedenti quindi si ha una correlazione rispetto a quanto fatto prima. Parliamo quindi della correlazione a doppio livello: la storia degli ultimi m branch viene utilizzata per prevedere cosa accadrà al branch corrente. Il branch corrente viene previsto con un predittore a n bit e in totale ci sono $2^m$ predittori a n bit. Il predittore effettivo per la previsione corrente viene selezionato sulla base dei risultati degli ultimi m branch,  codificati nella maschera di $2^m$ bit. Un predittore correlato a due livelli della forma (0,2) si riduce a un classico predittore a 2 bit.

![[predittori_correlati.png|400]]

---