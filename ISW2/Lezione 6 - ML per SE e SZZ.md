#is2 #magistrale 
[[Lezione 5 - Manutenibilità, debito tecnico e smell]]
le misurazioni che effettuiamo catturano il passato, e vengono utilizzate per il futuro.
assioma 1 nel ML : il futuro è simile al passato.


nella slide terminology:input le colonne sono le metriche. non tutti gli ambienti di ML supportano scale ordinali. in questi casi o si rimuove la colonna oppure si rilassa la scala in una binaria. le metriche rappresentano le feature. 3 metriche basilari sono: Density degli smell, LOC e churn

nella scelta delle metriche, bisogna scegliere metriche che sono utili dal punto di vista dell'ingegneria del software. si cercano metriche indipendenti tra loro ma correlati alle variabili (?)
sarebbe prassi avere un commit per un ticket di jira

spesso oltre la predizione ci serve anche una spiegazione.

principio del garbage in-garbage out :
ad ogni input scorretto segue un output scorretto.

l'idea di SZZ, è definire come sorgente o causa del difetto, la stessa zona impattata dalla correzione del difetto.
tutti gli elementi toccati da un commit del fix, prima del commit erano difettosi.
intendiamo revisione qualsiasi stato conseguente un commit.
la release è una revisione che è andata in produzione.

per prima cosa bisogna definire il dataset nel progetto.
vediamo come etichettare in modo euristico una classe come buggy o no
attraverso l'etichettatura potrei confrontare le tecniche di sviluppo per scoprire quale fornisce meno classi buggy.
N.B. tutto il ragionamento vale anche a livello di metodi o linea di codice.
noi misuriamo quando la classe è difettosa e non quando il difetto è stato inserito.

per avere un'idea del release id basta confrontare le date dei commit su jira con due release diverse, dove tutti i commit che vengono dopo la release 1.1 e prima release 1.2 appartengono allo stesso insieme di revisioni.

una classe è non difettosa (buggy) fino a prova contraria.
tramite szz stabiliamo se una classe è difettosa ma siamo imprecisi sul stabilire da quando.

un bug regressivo è un bug creato mentre si cercava di realizzare altro.
utilizzare git blame per identificare il commit che ha inserito il bug è ragionevole ma non è preciso.
senti registrazione.

una somiglianza tra un requisito attuale ed uno precedente significa che i requisiti toccheranno le stesse classi. inoltre posso studiare le semantiche delle classi con la semantiche dei requisiti.
voglio predirre quale classe verranno toccate dal requisito prima dell'implementazione del requisito. caso B

caso A slide. senti registrazione.

soluzione per refactoring focalizzato e assegnazione degli sviluppatori

---

