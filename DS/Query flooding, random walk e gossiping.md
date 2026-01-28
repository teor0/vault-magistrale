# Query flooding, random walk e gossiping
Nel query flooding il nodo richiedente P manda la query ai sui vicini che la ignorano se l'hanno già ricevuta, rispondono se possiedono la risorsa o altrimenti inoltrano la query ai loro vicini, eccetto P. La risposta può essere mandata direttamente dal proprietario della risorsa al richiedente (direct routing) oppure la risposta segue lo stesso percorso dalla query attraverso il query ID senza dover ricalcolare il percorso per l'instradamento, riducendo il rischio di cicli e instradamento bilanciato.
Una possibile ottimizzazione è utilizzare un Time-to-live (TTL), che decresce di 1 ad ogni hop fino a 0. Una seconda ottimizzazione è assegnare un ID univoco alla query in modo che i nodi non processino la stessa query più volte.
<font color=red>Svantaggi</font>: 
- overhead di comunicazione anche le query senza successo consumano rete. Costo O(N)
- nessuna garanzia che il proprietario della risorsa sia raggiunto.
- se i "vicini" sono fisicamente distante, si ha latenza elevata.
- vulnerabile ad attacchi DoS.

Nel random walk il nodo richiedente P manda la query ad un suo vicino scelto casualmente che risponde se possiede la risorsa oppure inoltra la richiesta ad uno dei suoi vicini sempre scelto casualmente. Un'ottimizzazione è il k-random walk dove, il nodo richiedente inizia k random walk indipendenti, mandando la query a k vicini casuali bilanciando il traffico. Ulteriori ottimizzazioni sono anche qui un TTL e un meccanismo per chiedere all'originator se serve ancora inoltrare la richiesta della risorsa o no.
<font color=red>Svantaggi</font>: 
- si hanno meno messaggi e meno congestione ma i tempi di risposta sono più grandi

I protocolli basati su gossip sono protocolli probabilistici dove ogni nodo invia messaggi ad un sottoinsieme di nodi della rete scelto casualmente. Tali protocolli sono semplici, non centralizzati, scalabili dato il numero limitato di messaggi scambiati e robusti grazie alla ridondanza dei messaggi. Gli approcci per la diffusione dei messaggi sono due: anti-entropy e rumor spreading.
Nell'anti-entropy ogni nodo periodicamente seleziona un altro nodo casualmente, per scambiare informazioni con l'obiettivo di raggiungere uno stato identico su entrambi i nodi. In particolare, un nodo P seleziona un nodo Q casualmente e sceglie una politica tra push, pull e push-pull, la politica più efficiente, per scambiare informazioni.
Nel rumor spreading, un nodo che ha un nuovo aggiornamento (è stato contaminato) periodicamente seleziona F>=1 nodi e invia il proprio aggiornamento. Tali F nodi possono poi scegliere se diffondere l'aggiornamento o meno. In particolare: un nodo P che ha un aggiornamento da comunicare contatta un nodo Q casuale e inoltra l'aggiornamento. Se Q ha già ricevuto l'appartamento, P potrebbe perdere interesse nel diffondere ancora l'aggiornamento, con probabilità $p_{stop}$ P smette di contattare altri nodi.

> [!important] 
> A parità di round, il gossiping congestiona meno la rete, raggiunge meno nodi rispetto al flooding e non da garanzie sulla copertura totale. Il flooding raggiunge copertura completa in meno tempo ma con maggiore congestione e messaggi ridondanti.

## Casi d'uso gossiping
Entrambi i casi d'uso sfruttano due parametri per controllare il gossiping: *B*: il numero massimo di vicini a cui il messaggio viene mandato e *F*: numero di volte in cui un nodo inoltra un messaggio ai suoi vicini.

<font color=purple>Blind counter rumor mongering</font>:
Un nodo *n* avvia una trasmissione inviando il messaggio *m* a B dei suoi vicini, scelti a caso. Quando il nodo *p* riceve un messaggio m dal nodo *q*, se *p* ha ricevuto *m* non più di F volte p invia *m* a B vicini, scelti in modo uniforme a caso, tra quelli che *p* ritiene non abbiano ancora visto *m*. 
Con B=F=2 rispetto al flooding si hanno il 50% di messaggi in meno, copertura del 90% ma disseminazione delle informazioni circa 2 volte più lenta.

<font color=purple>Bimodal multicast</font>:
Si basa su due fasi, la fase di distribuzione dei messaggi dove un processo manda in multicast un messaggio senza nessuna particolare garanzia di affidabilità. Nella fase di gossip repair, dopo aver ricevuto un messaggio un processo inizia a fare gossip con un insieme di nodi ad intervalli regolari dando ai processi la possibilità di confrontare i loro stati e colmare le lacune nella sequenza dei messaggi. È bimodale in quanto un messaggio è quasi sempre consegnato alla maggior parte o a pochi processi e quasi mai ad alcuni processi e perché la latenza di consegna dei messaggi è caratterizzata da: una distribuzione con latenze molto basse (per messaggi che arrivano senza perdite nella fase iniziale) e una seconda distribuzione con latenze più elevate (messaggi che hanno dovuto essere riparati in seguito)