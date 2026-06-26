#is2 #magistrale 
come abbiamo visto i criteri di adeguatezza danno un'idea su quanta parte del SUT sia stata esplorata. Non sono indici dell'effettiva capacità di fault detection della test suite.
è necessaria una differente categoria di criteri, basati sulla capacità di scoprire alterazioni artificialmente introdotte in un SUT è necessario per stabilire se l'insieme di test sviluppato è attendibile o meno.
I criteri quindi valutano la fault detection capability del mio insieme di test/test suite. Anche in questo caso possiamo avere alterazioni strutturali ed alterazioni funzionali. 
Parliamo quindi di mutation testing.
Partendo dall'assunzione che il programma sia corretto, stabiliremo se la test suite è adeguata attraverso l'introduzione di modifiche al SUT.
Partiamo da uno schema di processo ideale di mutation testing. 

risenti 13:30 per definizioni 
con L individuiamo l'insieme di mutanti (alterazioni artificiali)
con D individuiamo un insieme di "mutanti" scoperti dall'insieme di test. 
con E individuiamo i mutanti equivalenti, ovvero un SUT con un alterazione artificiale che non riesco a distinguere. è come se fosse una versione semanticamente alternativa. non è un'alterazione che porta a modifiche nel comportamento del SUT.
19:00 inizio spiegazione diagramma

una volta generati i mutanti ho l'insieme L, itero su ogni mutante ed eseguo un test. all fine considero se l'esito del test sul mutante è lo stesso del test sul programma originale P. se trovo un esito diverso ho rilevato un'alterazione, altrimenti valuto un altro test. una volta valutati tutti i test, se non trovo alcuna alterazione allora i test sviluppati non sono in grado di rilevare l'alterazione introdotta dal mutante. A fine del processo se L contiene dei mutanti ho due casi: la test suite non è abbastanza buona per rilevare tali alterazioni artificiali, oppure il mutante è equivalente. Con i mutanti equivalenti calcolo l'indice di fault detection.
n.b. identificare i mutanti equivalenti di per se è complesso intrinsecamente.

il mutation score è il numero di mutanti rilevati dall'insieme dei test T, rispetto al numero totale di mutanti rimasti non rilevati.
attenzione che L è sia il numero di partenza che il numero di mutanti rimasti vivi. per questo ${|D|\over |L|+|D|}$

può accadere che L sia con cardinalità elevata per via del fatto che le alterazioni generate sono troppo per i miei test
versione alternativa e più raffinata: numero di mutanti rilevati dall'insieme di test T rispetto al numero totale di mutanti generati ma non equivalenti al SUT 

dato un SUT ed un suo mutante M, se è rilevato allora tutte le seguenti condizioni sono valide:
1. raggiungibilità: esiste un'esecuzione concreta dal punto di attivazione di M allo statement mutato dal SUT
2. infezione dello stato: lo stato di M e quello del SUT differiscono a seguito dell'esecuzione dello statement mutato
3. propagazione dello stato: a seguito dell'attivazione dello statement mutato, l'impatto sullo stato di M si propaga fino al termine dell'esecuzione considerata.

>[!important] Mutante equivalente
M è considerato equivalente al SUT se **non esiste** un test case nel dominio di input del SUT in grado di soddisfare contemporaneamente le 3 condizioni.

risenti 58:30 per strong e weak mutation
esistono due approcci alla mutazione. strong e weak mutation

per PIT si possono utilizzare le metriche default oppure vedere come varia il report in base alla famiglia di operatori scelti

risenti 18:45 20/05
le mutazioni sono sempre a livello sintattico e non a livello funzionale e non c'è corrispondenza tra errori frutto dalla modificata sintattica ed errori "funzionali". gli errori introdotti non sono sufficientemente simili ai veri errori che si commettono 

27/05 recap fino a min 25
la popolazione iniziale è una possibile codifica dei casi di test. spesso si utilizzano i vettori come rappresentazione per gli algoritmi di mutazione.
un problema che si ha quando si mutano i test è che i test che si ottengono, vanno a massimizzare la funzione obiettivo, senza tenere conto in alcun modo delle funzionalità.
tali test diventano quindi molto complessi da manutenere

i test frutto di cross-over ha senso tenerli se producono una mutazione nel SUT.

vedi paper per più dettagli su teoria

evosuite tool per generazione del progetto.

da 01:00:00 recap scaletta progetto

i test che progettiamo devono passare tutti. solo in presenza di un bug che va motivato vanno ignorati i test che non passano.