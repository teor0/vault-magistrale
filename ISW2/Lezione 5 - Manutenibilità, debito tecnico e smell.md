#is2 #magistrale 

Il debito tecnico cerca di applicare i concetti finanziari al dominio dell'ingegneria del software. È ampiamente applicabile ma spesso è mal utilizzato. Il debito tecnico è un trade-off tra time to market e manutenibilità. Prima di dare più dettagli sul debito tecnico diamo alcune definizioni utili:
- La **regola di qualità** (quality rule) specifica come il codice deve essere, dunque è una regola sulle qualità interne. Alcune regole si rifanno ad un'entità come ad esempio il numero di metodi in una classe mentre altre no come la densità di commenti nel codice.
- Una **violazione** è una porzione di codice che non rispetta la regola di qualità ma ha impatto sulla qualità interna. La violazione è sinonimo di code smell.
- Un **difetto** è un problema nel codice che richiede dello sforzo (effort) per essere sistemato. Riguarda qualità esterne.
  
Tra le regole di qualità e le violazioni, possiamo affermare che vige una relazione molti a molti, dunque una o più violazioni corrispondono ad una o più regole. Questo lo vedremo anche in Jira e Sonarcloud. In generale il code smell è una violazione nell'ambito delle regole di qualità. Vedremo come il debito tecnico impatta la **prossima** release, inoltre più è manutenibile un sistema, più la release è facilmente sviluppabile. Possiamo pensare al debito tecnico come il risultato di un'ottimizzazione a breve termine che porta però a problemi sul lungo termine. Si intuisce perciò che il debito tecnico può emergere organicamente dato che ogni sistema durante la sua evoluzione e sviluppo diventa sempre più complesso, oppure può essere frutto di scelte arbitrarie: "rilasciamo adesso ed in futuro miglioreremo l'implementazione".

![[debito_tecnico.png]]

> Spesso la qualità esterna ha un peso maggiore sulla qualità interna, tuttavia la qualità interna impatta indirettamente sulle qualità esterne.

Il debito tecnico consiste in due concetti: l'ammontare del costo del debito e l'interesse sul debito. Una classe assai complessa può richiedere uno sforzo significativo per essere refactored (ammontare del debito), tuttavia se non avviene il refactoring potrebbe rallentare la velocità di sviluppo (interesse). Il refactoring è dunque quella attività in cui si eliminano i code smell o le violazioni e permette perciò di migliorare la qualità interna. Il debito tecnico è quindi molto vicino alla manutenibilità in quanto aggiunge tempo e criteri economici al processo di decisionale durante lo sviluppo. Tuttavia il debito tecnico scade con il tempo, questo perché il progetto software smette di essere manutenuto. Inoltre un contratto non include aspetti di manutenibilità a meno di vincoli specifici che nella maggior parte dei casi non ci sono. Il successo di un progetto dipende dagli obiettivi di business: costi, time-to-market o soddisfazione dei consumatori. Quindi è importante misurare i code smell ma non è fondamentale rimuoverli! Il debito tecnico va sicuramente monitorato ed analizzato ma non deve essere il fulcro dello sviluppo.

## Sonarcloud e caso di studio
Capire se, quali e quante regole di qualità è davvero importante per prevenire difetti durante manutenzione del software. 
All'interno del caso di studio ci aspettiamo che entità con più code smell sono più difficili da manutenere. Come metrica del caso di studio si usa il numero di difetti inseriti nella release calcolati a partire da un insieme di classi in cui all'inizio misuro il numero di smell e poi alla fine il numero di difetti introdotti nella release. A questo punto mi aspetto una correlazione positiva tra numero di smell e difetti. La correlazione non è lineare ma è normalizzata tramite la dimensione della classe e l'ammontare di manutenzione. Come output abbiamo numero di difetti diviso l'ammontare di codice modificato nella release come input abbiamo le violazioni e la dimensione della classe. 
![[studio_sc.png]] il grafico mostra la relazione con intervallo di confidenza al 90% tra la densità di violazioni e la defect injection frequency ovvero il numero di difetti diviso il numero di linee di codice modificate nell'entità. Si nota che con densità di violazione pari a 0, per una classe con 100 modifiche al codice, avrò 3 difetti alla fine. Per una densità di smell pari al 45%, la stessa classe dopo 100 modifiche avrà 21 difetti. In presenza di più code smell la varianza aumenta come si nota dal grafico.

## Caso di studio worst smell
La maggior parte del debito tecnico viene visto come un compromesso tra qualità e velocità di sviluppo. D'altronde alcune regole di qualità possono essere ignorate alle volte. La maggior parte dello sviluppo ed il costo è devoto alla manutenzione e come abbiamo visto i code smell portano a debito tecnico che riduce la manutenibilità ergo ci fa sprecare risorse. Esiste però un meccanismo che ci permette di evitare di introdurre smell nel codice, i quality gate.

> [!info] Quality gate
> Un quality gate è un meccanismo che non consente di inserire code smell nel codice in una commit andando a rifiutare la commit se non soddisfa le regole di qualità associate.

Tutta via i quality gate sono visti dagli sviluppatori come un rallentamento per il time to market. Andiamo quindi a capire tramite uno studio, quali smell vanno bloccati e che non portano perdite al time to market perché non hanno motivo di esistere. Per "worst smell" identifichiamo gli smell che se introdotti non portano ad alcun beneficio e non hanno alcun motivo giustificabile di esistere. La prova regina che si identifica nello studio è il lazy naming (Metodo1) che non si considera come un trade off strategico ma come un errore o svista. Si è visto poi come in termini assoluti i non-worst smells sono più comuni, tuttavia la probabilità che uno sviluppatore violi una specifica regola a "worst smell" è simile alla probabilità di violare una regola dei "non worst smell". Anche se il numero di occorrenze degli smell è correlato alle linee di codice modificate, la tipologia di smell introdotta non cambia l'ammontare di manutenzione richiesta. Tuttavia per la severità si è osservato il seguente risultato: ![[sev_ws.png]] dunque anche se sembrano avere una severità minore in media, i worst smell rappresentano più frequentemente le violazioni blocker. A termine dello studio si è cercato di spiegare il perché si introducono tali smell e le motivazioni sono diverse e non in ordine:
1. mancanza di conoscenza dello sviluppatore
2. commit di codice preliminare o di "test"
3. errore intenzionale
4. assunzioni improprie da un linguaggio ad un altro, ad esempio da Java a C
5. errore senza alcuna intenzione

In conclusione il 25.5% dei code smells non ha ragione di esistere, e sebbene abbiano la stessa frequenza rispetto agli altri smell, non offrono alcun beneficio a seguito di un trade-off. Al contrario, concentrandosi su questi smell, un progetto può avere più qualità senza perdere in agilità. Morale: il debito tecnico è una scelta, ma i "worst smell" sono solo errori.

---


