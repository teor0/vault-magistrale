#is2 #magistrale 
Per il progetto, è consigliato rivedere le linee guida viste a lezione.
Quali meccanismi bisogna mettere in piedi per garantire conformità alle specifiche nella progettazione e sviluppo del software? Meccanismi di verifica e validazione, ma non per realizzare il sistema, ma per accertarsi che lo sviluppo segua le specifiche.
Le attività di verifica e validazione possono essere partizionate in due classi: 
1. attività di verifica per comprovare che il prodotto che si realizza è il prodotto bene. Si seguono le specifiche tecniche prodotte dagli sviluppatori.
2. attività di validazione per comprovare che il prodotto è effettivamente quello che il committente ha richiesto, è il prodotto giusto. Si seguono le specifiche esterne determinate dagli stakeholders.

La differenza sta negli obiettivi e non come raggiungere gli obiettivi. Tra gli obiettivi delle attività di verifica e validazione c'è la rassicurazione dell'utilizzatore.

Sia $S$ una descrizione di un comportamento, cioè una specifica (spesso incompleta) da realizzare con un programma. Una descrizione che dichiara per gli input in $D$ quali sono gli output attesi in $C$. Un programma $P$ è un'implementazione corretta della specifica $S$ se e solo se $\forall d\in D\ P(d)=S(d)$.

Per dimostrare che il programma P effettivamente soddisfa la specifica $S$ su ogni input, utilizziamo la verifica formale. La verifica formale se è possibile applicarla va applicata. La compilazione corretta di un file sorgente è una verifica formale. Per la verifica formale necessitiamo di un team di esperti in diversi domini. Nelle prove di verifica si assume che il sistema è conforme alla rappresentazione non direttamente eseguibile, su cui effettuiamo le verifiche formali.
Se assumiamo che P abbia qualche imperfezione che non fa sovrapporre P a S, quindi passiamo a dimostrare che il programma P soddisfa la specifica $S$ su abbastanza input rappresentativi, utilizziamo il software testing e debugging. Le tecniche di testing hanno come obiettivo trovare eventuali differenze tra P e S, attraverso strategie pianificate. Le tecniche di debugging hanno come obiettivo individuare la causa della difformità tra P e S per rimuoverle. Il testing può essere fatto sulle specifiche dello stakeholder quindi con un'ottica di validazione e sulle specifiche di sviluppo con un'ottica di verifica.
L'errore è la causa del difetto che a sua volta è la causa di un malfunzionamento. 

Le tecniche di software testing sono tecniche in cui l'esecuzione di alcuni esperimenti, avviene in un ambiente controllato al fine di poter acquisire sufficiente fiducia sul suo funzionamento. Tipicamente avviene su aspetti funzionali e può riguardare anche caratteristiche extra-funzionali.
Il testing non può dimostrare l'assenza di guasti ma solo la loro presenza.
Solo con la verifica formale il sistema si può dire corretto.
Gli errori non sono sparsi ma localizzati in piccole aree del dominio.