#is2 #magistrale 
# Predirre le classi toccate - perché?
È stato fatto uno studio per predirre quale classe verranno toccate dal requisito prima dell'implementazione del requisito. Questo perché se sono a conoscenza di quale classe è stata toccata da quale ticket, potrei creare un modello per assegnare gli sviluppatori in modo da minimizzare il numero di nuovi sviluppatori che si occupano di una classe ed anche avere una soluzione per effettuare refactoring focalizzato su quelle specifiche classi.. Per capire quale classe è toccata dal requisito esistono diverse famiglie di metriche come i due casi riportati di seguito:
![[prediction_touched_class.png|300]]
Caso a, dato lo storico di una classe, posso vedere quali requisiti hanno interessato quella classe e preso un requisito corrente che è simile ad un requisito implementato dalla classe, allora ragionevolmente il requisito corrente interesserà la stessa classe del requisito già implementato. In questo caso si crea la semantica del codice a partire dai requisiti implementati dal codice specifico.
Oppure nel caso b, posso studiare le semantiche delle classi o meglio del codice e confrontarlo con la semantiche di un requisito e se simili posso dire ragionevolmente che il requisito interessa quel codice. ![[overview_prediction_touch.png| 300]] 
Ci sono poi gli smell, dato che molti smell indicano che una classe è stata toccata di più rispetto ad una con pochi smell, stessa cosa che accade secondo concetti di complessità, ovvero una classe più complessa è più probabile che sia toccata di una classe meno complessa. Infine abbiamo il TLCC (Temporal Locality of Class Changes), il grado di "calore" di una classe in quel determinato momento. Esistono classi che hanno un alto calore perché sono toccate sempre, oppure classi che sono toccate poco ogni tanto. Se una classe non è toccata da molto tempo ragionevolmente una classe non sarà toccata nel futuro prossimo. 

---
# Proportion

Come abbiamo visto SZZ, è un processo che identifica quando un difetto è stato introdotto in un progetto software attraverso il meccanismo di annotazione del sistema di versionamento in uso, ad esempio git blame. Quindi dato un commit di fix, l'algoritmo determina, per le righe del codice sorgente che sono state modificate, quando è avvenuta l'ultima modifica prima del fix. Abbiamo visto anche che questo processo è limitato. Attraverso un lavoro di ricerca è nato proportion come una nuova metodologia di etichettatura di classi difettose basata sul concetto di ciclo di vita di un difetto. In particolare le fasi del ciclo di vita di un difetto sono identificate come segue:

![[def_lifecycle.png]]

- la opening version (OV) è la versione in cui il ticket è aperto.
- la fix version (FV) è la prima versione del codice senza il bug riferito nel ticket.
- l'affected version (AV) è la versione che comprende l'introduction version fino al commit del fix tecnicamente.
- la introduction version/injected version (IV) è la versione in cui il bug è stato introdotto.

Una classe risulta buggy dalla injected version (inclusa) alla fix version (esclusa).
>Attenzione che l'inject version può coincidere con l'opening version, e se ciò avviene la fixed version è la versione successiva. 

Noi andremo a considerare i bug come post release. Attenzione anche alla consistenza tra affected version e opening version: se l'AV è dopo la creazione del ticket c'è un errore di consistenza. Si intuisce che la prima affected version, è proprio l'injected version.
L'opening version è sicuramente la prova più forte per la presenza della failure.

Per prima cosa lo studio però si fa una domanda più che legittima, ovvero se l'AV ed informazioni legate ad essa siano disponibili e consistenti, per stabilire se è affidabile da utilizzare per etichettare classi come difettose o meno al fine di creare il nostro dataset. Nello studio si analizzano 212 progetti di Apache andando a considerare i ticket del tipo
> Type == “Bug” AND (status == “Closed” OR status == “Resolved”) AND Resolution == “Fixed”

e ignorando i bug che non hanno un fix commit relativo e tutti i bug che **non sono** post-release. Il risultato dello studio è che in media, **circa il 30%** dei ticket non hanno una affected version associata. Si capisce dunque che non è sempre possibile utilizzare l'AV come "oracolo" data la grossa inconsistenza. 
Successivamente nello studio ci si è chiesti se le informazioni che si traggono dal ciclo di vita di un difetto, sono abbastanza accurate per etichettare AV piuttosto che utilizzare SZZ. Questo perché, sono necessarie un certo numero di versioni per trovare il bug ovvero arrivare alla OV, ed una volta individuato il bug, sono necessarie un certo numero di versioni per correggerlo, ovvero arrivare alla FV. 

> L'intuizione sta nel fatto che ci sarà un numero proporzionale di versioni per trovare il difetto e per sistemare il difetto! In particolare la proporzione tra introduction version e opening version è pari alla proporzione tra opening version alla fixed version, e siccome opening e fixed version le conosco sempre, posso stimare l'affected version.


Questo è il cuore di proportion, utilizzare informazioni da ticket completi per completare ticket incompleti sprovvisti di AV. Per stabilire ciò si sono utilizzati progetti con almeno 6 versioni, che hanno più del 50% di ticket con AV e che hanno almeno 100 bug che sono collegati ad un fix commit e che contengono un AV disponibile e consistente. Dei 212 progetti solo 76 soddisfano tali condizioni.
Si definisce $P={(FV-IV)\over (FV-OV)}$ e come introduction version prevista come $FV-(FV-OV)*P$. In particolare per calcolare P ci sono quatrro tecniche in base a quali ticket utilizzo:
- Cold Start: calcoliamo P come al mediana di ticket tra gli altri 75 progetti. Più in generale si utilizzano ticket di altri progetti per calcolare P in quanto magari, il progetto in esame non ha informazioni consistenti e disponibili per AV. Problema del cold start è che stiamo utilizzando altri progetti!
- Increment: si calcola P come la media tra i difetti corretti nelle versioni precedenti. Quindi se sono alla release 4, calcola P come la media su i ticket delle release 1, 2 e 3. Il limite di increment è che il progetto nel tempo cambia, quindi non ha molto senso considerare il passato se è molto diverso dal presente.
- Moving window: si calcola P come la media tra l'ultimo 1% di difetti corretti, perciò si usano ticket "freschi" che rappresentano il presente. Moving window premia la qualità dei dati, ma non si sa la dimensione adeguata della window.
- Total: utilizzo tutti i difetti con affected version per calcolare P mentre per i difetti che non hanno affected version utilizziamo il P calcolato. Perché usare total: ho pochi ticket e li uso tutti per avere dati più veri possibili. Perché non usare total: si rischia di etichettare in maniera irrealistica dato che si usano ticket futuri per etichettare ticket precedenti. Se preservo l'ordine dei dati ho un label meno accurata ma realistica, mentre se uso tutti i ticket ho una metrica più accurata ma non realistica.


> [!important] Problema di realismo di total
> Stiamo quindi sovrastimando le performance del classificatore! Non è realistico predirre qualcosa a release 3 utilizzando dati della release 1, 2 e 3. Bisognerebbe utilizzare solamente i dati delle release 1 e 2!

Nella pratica desideriamo un training set realistico e testing set accurato, dato che il testing set deve essere vero ed il training set deve dare informazioni ragionevoli al modello. I dati più veritieri possibili sono tutti i difetti, mentre i dati reali sono difetti fino a quel momento. Dunque sarebbe bene utilizzare l'approccio total per il testing set, mentre l'approccio incremental per il training set. Ciò significa che dovremmo avere due dataset etichettati in maniera diversa, uno per il training set ed uno per il test set. Per motivi di tempo useremo l'approccio total per il progetto.

Tornando allo studio che trattiamo, in RQ2 per stabilire se è più accurato proportion o SZZ nello studio, si vanno a considerare diverse metodologie:
- Simple: si assume che IV è lo stesso di OV
- SZZ_B, SZZ_U, SZZ_RA
- Tutte le varianti di proportion
- +: tutte i metodi SZZ combinati con simple

In particolare SZZ_B va al primo blame SZZ_B+ va al primo blame oppure alla opening version. SZZ_RA ignora i cambiamenti identificati come refactoring e va una commit più indietro fino a che non trova una che modifica le funzionalità. Poi si sono presi tutti i ticket in cui la injected version era presente e consistente e per ogni ticket si è andati a predirre l'injected version con le diverse metodologie, utilizzando come unità di misura una versione. Di seguito i risultati ![[prop_rq2_results.png|300]] che mostrano come proportion è più precisa di SZZ.
![[prop_stdv.png|300]] P è abbastanza stabile su bug di diversi progetti, se confrontati con IV, OV e FV. Questo perché i bug condividono lo stesso ciclo di vita in termini di proporzione del numero di versioni tra il fix e la scoperta, soprattutto all'interno dello stesso progetto. 
Passando ad RQ3 dove non lavoriamo con unità di misura la versione ma la classe i risultati sono i seguenti: ![[prop_rq3.png|300]] si capisce che in generale proportion ha metriche di performance migliori di SZZ tranne che per il recall. Ma perché recall è più alta in SZZ e non in proportion? Perché SZZ tende a trovare più positivi di proportion dato che va più indietro nel tempo!


l'approccio di proportion è piuttosto biased

tutte le classi nascono non difettose. per avere una classe buggy necessito di una prova, ovvero un id di un ticket per un commit sulla classe.
in szz se vado troppo indietro creo dei falsi positivi. un bug regressivo szz lo etichetta come falso positivo, ovvero una classe che viene toccata dal commit con id di un ticket che però non c'entra nulla, verrà etichettata come buggy.
ho falsi negativi se nel commit non ho l'id del ticket.

cross-project estimation per il cold start.

Abbiamo due approcci: szz+proportion se non abbiamo l'AV oppure szz+la prima affected version/fix version in Jira. L'approccio con szz + affected version è la migliore combinazione.

---
