#is2 #magistrale 

Il debito tecnico cerca di applicare i concetti finanziari al dominio dell'ingegneria del software. È ampiamente applicabile ma spesso è mal utilizzato. Il debito tecnico è un trade-off tra time to market e manutenibilità. Prima di dare più dettagli sul debito tecnico diamo alcune definizioni utili:
- La **regola di qualità** (quality rule) specifica come il codice deve essere, dunque è una regola sulle qualità interne. Alcune regole si rifanno ad un'entità come ad esempio il numero di metodi in una classe mentre altre no come la densità di commenti nel codice.
- Una **violazione** è una porzione di codice che non rispetta la regola di qualità ma ha impatto sulla qualità interna. La violazione è sinonimo di code smell.
- Un **difetto** è un problema nel codice che richiede dello sforzo (effort) per essere sistemato. Riguarda qualità esterne.
  
Tra le regole di qualità e le violazioni, possiamo affermare che vige una relazione molti a molti, dunque una o più violazioni corrispondono ad una o più regole. Questo lo vedremo anche in Jira e Sonarcloud. In generale il code smell è una violazione nell'ambito delle regole di qualità. Vedremo come il debito tecnico impatta la **prossima** release, inoltre più è manutenibile un sistema, più la release è facilmente sviluppabile. Possiamo pensare al debito tecnico come il risultato di un'ottimizzazione a breve termine che porta però a problemi sul lungo termine. Si intuisce perciò che il debito tecnico può emergere organicamente dato che ogni sistema durante la sua evoluzione e sviluppo diventa sempre più complesso, oppure può essere frutto di scelte arbitrarie: "rilasciamo adesso ed in futuro miglioreremo l'implementazione".

![[debito_tecnico.png]]

> Spesso la qualità esterna ha un peso maggiore sulla qualità interna, tuttavia la qualità interna impatta indirettamente sulle qualità esterne.

Il debito tecnico consiste in due concetti: l'ammontare del costo del debito e l'interesse sul debito. Una classe assai complessa può richiedere uno sforzo significativo per essere refactored (ammontare del debito), tuttavia se non avviene il refactoring potrebbe rallentare la velocità di sviluppo (interesse). Il refactoring è dunque quella attività in cui si eliminano i code smell o le violazioni e permette perciò di migliorare la qualità interna. Il debito tecnico è quindi molto vicino alla manutenibilità in quanto aggiunge tempo e criteri economici al processo di decisionale durante lo sviluppo. Tuttavia il debito tecnico scade con il tempo, questo perché il progetto software smette di essere manutenuto. Inoltre un contratto non include aspetti di manutenibilità a meno di vincoli specifici che nella maggior parte dei casi non ci sono.

è importante misurare i code smell ma non è fondamentale rimuoverli.

la classificazione in blocker, high, medium ecc. in sonarcloud rappresentano l'interesse del debito tecnico?
vediamo un caso di studio.
classi più manutenibili
entità con più code smell sono più difficili da manutenere,

in realtà la violations size è una densità non un conteggio slide 22.
in presenza di più code smell la varianza aumenta.

alcuni code smell non portano ad alcun beneficio, sono solamente errori.
un quality gate è un meccanismo che non consente di inserire code smell nel codice in una commit 
regole di qualità che non danno alcun vantaggio possono essere ignorate.
worst smell inteso come smell che non offre nessun vantaggio nel rispettare la regola di qualità associata.

