Nel caso di Zookeeper, tramite l'api di Jira tra le versioni che si ottengono vi sono alcune senza release date. Di seguito un esempio che mostra una versione con release date ed una versione priva del campo. Si è dunque scelto di rimuove tali versioni che non presentano il campo in quanto inutilizzabili.
![[confronto_versioni.png|300]] si passa così da 67 versioni a 61. Vado ad ignorare il 40% per avere un po' più di release a disposizione.

Come specificato nella documentazione:

>The Apache ZooKeeper community supports two release branches at a time: **stable** and **current**. 

Dunque vengano rilasciate versioni di fix per il branch: 3.x nel frattempo che il branch current sia il 3.x+1
Il fix della 3.x.y viene poi aggiunto per la versione 3.x+1.z
Ricordando che per Jira, l'affected version coincide con la injected version, ovvero la versione che introduce il bug, prendiamo ad esempio questo ticket:
![[ex_fix_jira.png]] il bug viene segnalato per la 3.4.6, 3.5.0 e 3.5.1 le fix version sono la 3.4.7, 3.5.2 e 3.6.0.
Lasciando stare la 3.6.0 abbiamo come fix version la  3.4.7 e 3.5.2. 
La 3.4.7 è la prima release che ha release date (03/12/2015) successiva a tutte le affected version e alla data di creazione del ticket (04/08/2015). Inoltre, andando a vedere le release date la 3.4.7 viene prima della 3.5.2 perciò si conclude che la fix version "master" è la 3.4.7 che risolve il bug il branch 3.4.6, mentre la 3.5.2 andrà a porre tale fix per i branch 3.5.0 e 3.5.1 
in conclusione la affected version più "anziana" è la 3.4.6 mentre la fix version 3.4.7 è quella "master" ovvero la prima versione in cui fix è rilasciato. Nella 3.5.2 banalmente si effettua "merge" del fix fatto in 3.4.7


In conclusione mi mantengo tutte le versioni che ho trovato tramite il codice. 

Tramite il comando `git rev-list -1 --before="2009-02-13T00:00:00" HEAD` ottengo l'hash degli ultimo commit effettuato prima di un certo cutoff. Come cutoff si è scelto di considerare la data di rilascio di una versione, nel esempio il 13/02/2009 è la data di rilascio della release 3.1.0. Quindi l'ultimo commit di una release è quello fatto entro il giorno prima di una nuova release.
Release che non possiedono tale commit verranno scartate.

Le metriche sono calcolate a partire dalla release 0 e non within release perché un file che viene modificato pesantemente in tutta la sua vita è più probabile che sia buggy. Insomma, diciamo che "non vi è assenza di memoria".