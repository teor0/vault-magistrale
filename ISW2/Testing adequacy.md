ai fini del corso non è richiesto andare a fare debugging per localizzare il bug, ma è sufficiente leggere la documentazione per capire se il test è corretto o meno (approccio white box) e quindi se si è trovata una failure oppure se si è sbagliati a codificare il test. ricorda che è necessario motivare se il test è fatto bene o meno.
se il test trova effettivamente successivamente si dovrà ignorare nel processo di testing di mvn.
inizio lezione a 29:00

ci chiediamo adesso se i test che si sono progettati ed implementati, siano sufficientemente buoni e adeguati
le tecniche di adeguatezza sono molteplici e si caratterizzano in base al tipo do:
- processo
- artefatti che usano/assumano
- strategie applicate

In generale non esiste una tecnica perfetta ed applicabile per il caso generale, dato che il testing è sempre un'attività best effort.

I criteri di adeguatezza si dividono in funzionali e strutturali.

I criteri di adeguatezza funzionali, ignorano l'interno del sistema, e traggono conclusioni dalle funzionalità indipendentemente dall'implementazione (approccio black box). Si considerano dunque i comportamenti osservabili, cioè ci si basa su modelli o specifiche del SUT che non sono direttamente eseguibili. Cosi facendo però non scopro errori dovuti a dettagli implementativi. Prendiamo ad esempio bookkeeper grazie alla documentazione, potrei trarre dei criteri di adeguatezza funzionali diversi in base alle specifiche descritte nella documentazione. Ad esempio, nella documentazione è specificato che il numero massimo di failure possibili è $Q_a-2$, dunque un possibile criterio di adeguatezza funzionale è: _almeno un test di scrittura-lettura su una entry che simuli di fallimento di $Q_a-2$ bookies_. 
Un altra possibilità è trasformare una specifica da informale a formale con ad esempio un activity diagram in UML e poi trarre dei criteri di adeguatezza da esso. 

I criteri di adeguatezza strutturali, (white box) traggono conclusioni in base all'implementazione. In questo caso ci si basa sull'effettiva implementazione del SUT che verrà eseguita, che esso sia codice o altre astrazioni che forniscono una rappresentazione effettiva ed eseguibile del SUT.
Si va a considerare la maggior parte del codice possibile, ma non è possibile rilevare errori su funzionalità mancanti! Potrei dunque avere delle buone metriche sulle funzionalità presenti, ma non su quelle assenti. Tramite la tecnica strutturale non mi accorgerei mai delle funzionalità mancanti. 

Attenzione, non c'è nessuna garanzia per cui alta coverage strutturale implichi esposizioni di failure perché, 'aver esplorato la maggior parte possibile del codice, aumenta solo la nostra confidenza con il SUT. E perché non c'è nessun legame con le funzionalità o i modi d'uso.
Paradossalmente un criterio di adeguatezza potrebbe essere il dover testare fino al secondo venerdì del mese il SUT.

Vedi slide per esempi di criteri di adeguatezza strutturali.
i criteri di adeguatezza strutturali fanno riferimento al linguaggio e non sul dominio. questo facilità l'automazione e la realizzazione di tool annessi.


slide 27 l'indici dei nodi è riferito ai blocchi di computazione della slide 28, non alla righe di codice.

il programma viene visto come un grafo. 
vediamo i criteri basati sul control-flow. per facilità li vediamo dal punto di vista white box, ma è possibile anche strutturarli in maniera black box. il primo è il statement coverage: copertura dell'insieme di test T rispetto alle istruzioni presenti nel SUT. 

T è adeguato per SUT se la sua statement coverage è 1. in altre parole alla fine di tutti i test non esiste uno statement che non sia stato eseguito almeno una volta.
spesso si usa considera T accettabile per SUT se la sua statement coverage è almeno $x\%$ dato che è impraticabile ottenere uno statement coverage del 100%.
questo perché esiste anche codice che è irraggiungibile. 

In generale diremo che: 

> T è adeguato per il SUT se la coverage del criterio è 1. T è accettabile per il SUT se la coverage del criterio è almeno $x\%$

block coverage:
invece di considerare istruzioni singole, considero blocchi di codice. il senso del criterio di adeguatezza resta il medesimo.


---
13/05
il branch decision coverage cerca di stimare quanto approfonditamente ho esplorato i punti di decisione presenti nel SUT.

la branch decision coverage non da informazioni sul perché si è scelto un certo branch decisionale, ma su quale si è esplorato.

la condition coverage è la copertura dell'insieme di test T rispetto alle espressioni condizionali (semplici/atomiche o composte) presenti nel SUT.

le condizioni composte, sono validate se T include casi di test che validano ogni singola condizione componente.

problema della condition coverage è che in Java come in C++, gli operatori logici sono cortocircuitati, dunque alcune condizioni non verranno mai considerate. Si va quindi a rilassare il vincolo sulle condizioni semplici del condition coverage. per ottenere un condition coverage utilizzabile.

condition coverage non implica branch decision coverage e branch decision coverage non implica condition coverage

branch condition coverage è l'evoluzione naturale dei due criteri.

esiste poi la multiple condition coverage, che esplora tutte le possibili combinazioni di espressioni condizionali semplici presenti nel SUT. Ovviamente nella pratica questo criterio non è praticabile dato che un sistema ha moltissimi parametri

il modified condition/decision coverage (MC/DC) è la copertura congiunta dell'insieme di test T rispetto a tutti i punti di scelta/decisione e tutte le espressioni condizionali semplici presenti nel SUT ma evitando l'esplosione combinatoria degli stati. 
Se ottengo le seguenti condizioni:
- ogni blocco nel SUT è coperto da T
- ogni condizione semplice nel SUT è coperta sia con true che con false in T. anche qui per lazy evaluation posso valutare l'OR


senti 01:01:00

Jacoco è il framework di QA che utilizzeremo nel progetto. Sono sufficienti le metriche di base per il progetto.

quindi sviluppo i test manuali con partition e boundary analysis 01:13
Jacoco lo utilizzeremo per valutare i test manuali, con randoop e quelli del LLM.
jacoco produce un file binario .exec va poi prodotto un report da tale file. in caso di problemi è consigliato vedere le linee guida di sonarcloud per multi-module projects vedi documentazione ed esempi sul pdf.