#is2 


> [!info] Definizione architettura software
> L'organizzazione fondamentale di un sistema, incarnata dai suoi componenti, le loro relazioni tra di loro e con ambiente e, i principi che ne regolano la progettazione e evoluzione. 

Kruchten afferma che :
> L'intento principale dell'architettura software è fornire controllo su un sistema sofisticato di enorme complessità. 

Ogni sistema ha una ed una sola architettura che può essere pianificata, dove si progetta il sistema e dopo si scrive il codice e si documenta, o spontanea dove prima si scrive il codice che viene riscritto di frequente per dar forma all'architettura. 

Sempre Kruchten definisce il principio cardine dell'architettura software:
> La separazione delle interessi e dei punti di vista (4+1) smonta la complessità del sistema consentendo una migliore gestione.

![[sa4+1.png]]

Il punto di vista logico si concentra sul supportare le funzionalità utente e i requisiti.
Il punto di vista di sviluppo si concentra su i moduli del codice sorgente.
Il punto di vista dinamico si concentra su i flussi di dati ed i processi/oggetti a run-time
Il punto di vista di deployment si concentra sulle configurazione, la distribuzione geografica ed il mapping del software su i server. 

Esempio di punto di vista logico: ![[logicview.png]]
Esempio punto di vista di sviluppo: ![[devview.png]]

Le metodologie di progettazione cercano di derivare l'architettura software dai requisiti funzionali e di qualità. Ci sono 3 principali fattori che guidano la progettazione:
- riuso: la progettazione è un compito difficile, in cui il riuso può semplificare tale compito. La "fonte" del riuso possono essere componenti di un sistema già progettato per lo stesso dominio. La Product Line Architecture è un esempio di progettazione orientata al riuso.
- metodi: esistono numerosi linguaggi, modelli di processo e metodi che prescrivono una tecnica sistematica per colmare il notevole divario tra architettura e requisiti del software. 
- intuizione: molti progettisti si affidano all'invenzione di elementi di progettazione ed alle loro relazioni basate esclusivamente sulla propria esperienza.
  
La proporzione di questi 3 elementi dall'esperienza dei progettisti, il loro background e la "novità" del sistema. L'architettura di un sistema impatta su gli aspetti qualitativi del sistema. z

Il processo di progettazione dell'architettura software si concentra sulla derivazione di un'architettura dai requisiti software, e
come già accennato, comprende una tecnica decisionale. Le decisioni di progettazione sono il nucleo dell’architettura software. La vita di un architetto del software è una lunga, a volte dolorosa, successione di decisioni **non ottimali** prese in parte nell'**oscurità**.
L'attività del progettista è lunga perché si continua a progettare il sistema durante tutta la vita di esso. Talvolta i requisiti non sono chiari, ma le decisioni devono essere prese lo stesso ecco perché sono prese in parte nell'oscurità. Le decisioni sono non ottimali perché non tutti gli interessi degli stakeholder possono essere soddisfatti, ecco perché a volte sono dolorose, perché non rendo tutti "felici".

I requisiti non funzionali (ovvero gli attributi di qualità) sono difficili da specificare in un documento modello architetturale e sono onerosi durante lo sviluppo del sistema. Diamo adesso alcune definizioni utili per documentare un'architettura software.
- La descrizione dell'architettura è organizzata in una o più viste architetturali.
- Un modello architetturale, è una parte di una vista architetturale. Un modello architetturale può partecipare a più di una viste per consentire la condivisione di dettagli relativi agli interessi attraverso le viste, senza ripetizioni.
- un punto di vista architetturale, può essere visto come un insieme di interessi di più stakeholder, che riassume gli aspetti architetturali di rilevanza. Tali aspetti possono essere gli stessi su sistemi diversi.
- un vista architetturale, è l'applicazione del punto di vista sul sistema ed un insieme di modelli. Per ogni punto di vista nella descrizione dell'architettura, esiste un'unica vista. Ogni vista poi deve essere "governata" da esattamente un punto di vista architetturale.

Una descrizione architettonica può includere anche altri elementi architettonici informazioni che non sono adeguatamente contenute in nessuno dei suoi viste architettoniche, come risultato della documentazione di un’organizzazione pratiche.

![[schema_falessi.png]]

--- 
# Software Product line 
Ricordiamo che il costo di produzione del software non aumenta linearmente con il numero di volte che lo replichiamo, in quanto la replicazione del software è meno costosa. Tenendo a mente ciò, trattiamo la software product line, che prende ispirazione dalla sua "musa" utilizzata nel settore automobilistico. Prima però definiamo cos'è un architettura di riferimento.

> Un architettura di riferimento è essenzialmente, un modello o un insieme di modelli architetturali predefiniti, possibilmente parzialmente o completamente istanziati.

A livello astratto la meta-architettura o architettura di riferimento non va confusa con lo stile di architettura. Nell'ambito automobilistico, l'architettura di riferimento è il progetto base della macchina, che ogni costruttore bene o male riutilizza per più modelli, ad esempio l'Audi A1, A2 e A3 condividono quella che ormai viene definita, "piattaforma". Si ha quindi una linea di produzione per tali auto che condividono molti pezzi e variano magari nella cilindrata. In questi sistemi in cui il costo è dato sulla materia prima, si fa di tutto per risparmiare su progettazione e test. Nell'ingegneria del software però il costo è solo su progettazione e test, nasce dunque il concetto di linea di prodotto. 

Una linea di prodotti software (software product line) è un insieme di sistemi software, che condividono un insieme comune e gestito di funzionalità che soddisfano le esigenze specifiche di un particolare segmento di mercato e che si sviluppano a partire da un insieme di principi comuni in modo prescritto.


La linea di prodotto è una famiglia di prodotti progettati dove si sfrutta come vantaggio gli aspetti comuni tra i prodotti. Il riuso nella linea di prodotto è sistematico, dunque l'ammontare di riuso predetto, è la motivazione per l'investimento necessario a priori. Ma il riuso non riguarda solo il codice, nella linea di prodotto il concetto di variabilità è applicato ai requisiti, ai componenti del modello, ai test, ecc. Dunque si fa riuso di tutto, dalla documentazione, ai piani di test, ai test cases, alle configurazioni ecc. Il riuso poi si suddivide in pro-attivo e reattivo. Il riuso reattivo è quando ho una funzionalità da sviluppare e $n$ già sviluppate e mi chiedo cosa riutilizzare dalle $n$ sviluppate, mentre nel riuso pro-attivo sviluppo un componente sapendo che sarà riutilizzato in futuro.

Un fattore chiave per sfruttare al meglio le opportunità di riuso, è che gli investimenti nel riuso siano concentrati sulle funzionalità che promettono un ritorno ottimale del investimento.

Nel momento in cui ho requisiti di prodotti diversi, questi vengono analizzati insieme e viene definito un dominio tramite l'attività di scoping in cui scopro quali differenze e comunanza ci sono nei requisiti. Tramite astrazione, vado quindi a definire cosa deve essere sviluppato per un prodotto in maniera individuale, e cosa va sviluppato per il riuso. Tramite il family engineering, vado a sviluppare quei componenti che saranno riutilizzati per una famiglia di prodotti, dopo di che nel application engineering prendo quanto sviluppato in maniera astratta e la vado a personalizzare per un prodotto. Una volta istanziati i prodotti, avrò dei feedback che posso utilizzare per migliorare e perfezionare il prodotto o la famiglia stessa. Si capisce dunque, che i sistemi a linea di prodotto dovrebbero avere la stessa architettura di riferimento.

I principali svantaggi della product line sono:
Interfaccia complessa: un componente deve presentare un'interfaccia per essere esteso a specific prodotti. Anche se idealmente questa l'interfaccia è pulita e rivela dettagli minimi, in pratica, per supportare diverse estensioni, l'interfaccia diventa complessa.
Organizzazione caotica: un certo livello di riutilizzo può richiedere pesanti cambiamento organizzativi. Ciò potrebbe provocare problemi associati a produttività e prevedibilità del programma, motivazione, spirito di squadra, ecc.

Infine gli stili architetturali sono diversi e hanno obiettivi diversi tra loro come ad esempio: client/server, peer to peer o pipe and filters. Insomma non esistono anche in questo caso silver bullet (soluzioni perfette).

---



