Il progetto sarà Zookeeper (vedi pdf milestone 1). Il dataset è un csv che richiede il nome della classe, la release ID che può essere nome o numero, meglio numero. Circa 20 colonne + colonnna numero smells + colonna bugginess. 
Ad esempio LOC per release, LOC touched, ovvero la somma di LOC toccate da un commit per una release, ad esempio per 5 commit è la somma dei commit per quella release.
NR è il numero di commit
Nfix è il numero di commit che fixano un difetto.
ecc.
Vedi class metrics nel pdf per le altre colonne.
Le metriche possono essere misurate in due modi: o dalla release 0 oppure all'inizio dell'ultima release
ci sono anche metriche relative ai commit. ricordiamo che un commit che tocca più classi è più complesso intrinsecamente, dunque porta a più possibili difetti. se un commit è buggy ovviamente anche quella classe toccata dal commit sarà buggy. c'è ovviamente una relazione. potrei quindi mettere i commit relativi alla classe nel dataset. quanto grande era il commit, quante classi ha toccato il commit.
non avere timore di sbagliare
prima si calcolano le metriche e dopo l'etichettatura, dovrebbe essere più semplice e sensato.
registrazione 39:30 per indicazioni sul workflow! in aggiunta al workflow, c'è la possibilità di normalizzare con $log_10$ le colonne
trovato l'ultimo commit della release faccio il checkout per calcolarmi le caratteristiche. su quel checkout voglio predirre la bugginess
per il labeling assumiamo tutte le classi non buggy all'inizio e applico proportion total su tutte le release per stabilire quali classi sono buggy o meno. 
viene fornito il codice per identificare le release in jira e ticket buggy in jira come specificato nel pdf. poi si fa la query su git per trovare i commit con quel id, tutte le classi con quel commit saranno buggy dal IV fino all AF. la IV la trovo con proportion oppure con il ticket come visto nelle lezioni.
ad esempio che la AF trovata o ricavata è 3. la FV che c'è magari è 5, le classi toccate dal commit saranno nel dataset cambiate in buggy:yes per le versioni 3 e 4. risenti 45:55 per esempio.
ovviamente c'è correlazione tra le feature ma vedremo più avanti come fare feature selection.

alla fine del progetto faremo una predizione su quanti difetti osservati sarebbero potuti essere evitati per avere un codice perfetto.