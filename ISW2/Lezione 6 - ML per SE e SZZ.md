#is2 #magistrale 
[[Lezione 5 - Manutenibilità, debito tecnico e smell]]
# Machine learning per l'ingegneria del software
> Assioma 1 nel machine learning : il futuro è simile al passato.

Infatti le misurazioni che effettuiamo catturano il passato, e vengono utilizzate per il futuro. Dopo un ripasso sui concetti principali diamo definizioni di terminologia:
Istanze: gli esempi individuali ed indipendenti di un concetto che va appreso, ovvero le osservazioni. Ad esempio una classe, un metodo o giorni. Feature/attributi: metriche di misurazione per un'istanza ad esempio le linee di codice. Noi ci soffermiamo su scale nominali che consentono solo confronti di uguaglianza e numeriche che consentono più operazioni.

Una tipica tabella popolata da istanze e feature è la seguente:
![[tab_ml.png]] tale tabella sarà data in pasto al modello di machine learning per predirre se una classe è buggy o no. Le colonne rappresentano le metriche, ma non tutte le metriche finiranno nelle colonne. Nel nostro caso una riga rappresenterà una combinazione classe-release. Non tutti gli ambienti di ML supportano scale ordinali, in questi casi o si rimuove la colonna oppure si rilassa la scala in una binaria. Di sicuro la dimensione della classe e quante volte una classe è stata modificata è correlato a quanto una classe è difettosa. Nella scelta delle metriche, bisogna scegliere metriche che sono utili dal punto di vista dell'ingegneria del software. 3 metriche basilari sono: densità degli smell, LOC e churn. Si cercano metriche indipendenti tra loro, ma correlate alle istanze. E spesso oltre la predizione sarebbe ideale anche una spiegazione!

> [!important] Ticket Jira
> Sarebbe prassi avere un commit per un ticket di Jira per aiutare ad identificare i difetti e soluzioni ad essi.

> [!important] Principio del garbage in-garbage out
> Ad ogni input scorretto segue un output scorretto.

Come detto la previsione dei difetti mira a identificare artefatti software che potrebbero presentare un difetto, al fine di ridurre i costi di test e revisione del codice, consentendo agli sviluppatori di concentrarsi su artefatti specifici. In definitiva, l’affidabilità di un modello di previsione dipende dalla qualità del set di dati. Dunque per prima cosa bisogna definire il dataset nel progetto in maniera adeguata. 

---
# SZZ

Vediamo come etichettare in modo euristico una classe come buggy o no cosa molto utile dato che ad esempio, attraverso l'etichettatura potrei confrontare le tecniche di sviluppo per scoprire quale fornisce meno classi buggy. La tecnica che vediamo è SZZ.
L'idea di SZZ, è definire come sorgente o causa del difetto, la stessa zona impattata dalla correzione del difetto. Dunque tutti gli elementi toccati da un commit del fix, prima del commit erano difettosi. Possiamo definire quindi SZZ come, un processo per identificare quando si è verificato un difetto effettivamente introdotto in un progetto software, sfruttando l'annotazione del sistema di source control. Utilizzare git blame per identificare il commit che ha inserito il difetto è ragionevole ma non è preciso. E vedremo in futuro come essere più precisi tramite proportion.

![[szz.png]]
Come si nota dalla figura, l'obiettivo di SZZ è determinare, per il codice sorgente che è stato modificato per correggere un difetto, quando è avvenuta l'ultima volta una modifica prima di tale correzione, che non è lo stesso momento in cui il difetto è stato rilevato e comunicato tramite un ticket di Jira. Per questo motivo è anche importante fare una distinzione tra revisione e release. Intendiamo revisione qualsiasi stato conseguente un commit, mentre la release è una revisione che è andata in produzione. SZZ come già anticipato non è una tecnica molto precisa ed ha diversi limiti quali: 
- bug regressivi 
- imprecisione nella ricerca all'indietro: stabiliamo se una classe è difettosa ma siamo imprecisi sul stabilire da quando.
- refactoring
- snoring: che vedremo cos'è in futuro.


> [!important] Nota bene
> Tutto il ragionamento vale anche a livello di metodi o linea di codice e attenzione, noi misuriamo quando la classe è difettosa e non quando il difetto è stato inserito.

> [!info] Title
> Per avere un'idea del release id basta confrontare le date dei commit su jira con due release diverse, dove tutti i commit che vengono dopo la release 1.1 e prima release 1.2 appartengono allo stesso insieme di revisioni.

Ricorda una classe è non difettosa (buggy) fino a prova contraria. Più piccolo è il cambiamento più l'eventuale bug è facile da risolvere. Ma se faccio migliaia di commit, lo stato del codice risulta piuttosto inconsistente. Dunque l'approccio un commit per ticket è ragionevole. Nel caso di ticket grandi invece potrei scomporli.

---

