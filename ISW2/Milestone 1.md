Il progetto sarà Zookeeper (vedi pdf milestone 1). 
Il dataset è un csv che richiede il nome della classe, la release ID che può essere nome o numero, meglio numero. Circa 20 colonne + colonnna numero smells + colonna bugginess. 
Ad esempio LOC per release, LOC touched, ovvero la somma di LOC toccate da un commit per una release, ad esempio per 5 commit è la somma dei commit per quella release.
NR è il numero di commit
Nfix è il numero di commit che fixano un difetto.
ecc.

Le metriche possono essere misurate in due modi: o dalla release 0 oppure all'inizio dell'ultima release
ci sono anche metriche relative ai commit. ricordiamo che un commit che tocca più classi è più complesso intrinsecamente, dunque porta a più possibili difetti. se un commit è buggy ovviamente anche quella classe toccata dal commit sarà buggy. c'è ovviamente una relazione. potrei quindi mettere i commit relativi alla classe nel dataset. quanto grande era il commit, quante classi ha toccato il commit.
non avere timore di sbagliare
prima si calcolano le metriche e dopo l'etichettatura, dovrebbe essere più semplice e sensato.
registrazione 39:30 per indicazioni sul workflow! in aggiunta al workflow, c'è la possibilità di normalizzare con $log_10$ le colonne.

trovato l'ultimo commit della release faccio il checkout per calcolarmi le caratteristiche. su quel checkout voglio predirre la bugginess
per il labeling assumiamo tutte le classi non buggy all'inizio e applico proportion total su tutte le release per stabilire quali classi sono buggy o meno. 

preso zookeeper. trovo tutte le release e le date. di queste release e date butto il 66% come visto nella teoria. per ogni release nel 34% trovo l'ultimo commit. faccio il checkout dello stato dell'ultimo commit in quella release (che rappresenta l'ultima versione modificata del codice, in quella release) quindi se la data della release da Jira è il 18 maggio prendo l'ultimo commit del 18 maggio faccio checkout e mi calcolo le caratteristiche.



il codice fornito permette di identificare le release e ticket di tipo buggy in Jira. poi bisogna fare la query su git per trovare il commit con quel id. ovvero il labelling. poi si fa la query su git per trovare i commit con quel id, tutte le classi con quel commit saranno buggy dal IV fino all AF. la IV la trovo con proportion oppure con il ticket come visto nelle lezioni.
ad esempio che la AF trovata o ricavata è 3. la FV che c'è magari è 5, le classi toccate dal commit saranno nel dataset cambiate in buggy:yes per le versioni 3 e 4. risenti 45:55 per esempio. fino a 49:40
ovviamente c'è correlazione tra le feature ma vedremo più avanti come fare feature selection.

alla fine del progetto faremo una predizione su quanti difetti osservati sarebbero potuti essere evitati per avere un codice perfetto.