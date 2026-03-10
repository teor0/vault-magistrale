#is2

# Metriche e misurazioni
L'ingegneria del software descrive l'insieme delle tecniche che applicano un approccio ingegneristico alla realizzazione e al supporto dei prodotti software. Per “approccio ingegneristico” si intende che ogni attività è compresa e controllata, in modo che ci siano poche sorprese durante la specificazione, la progettazione, la realizzazione e la manutenzione del software. Alla fine della fiera però questo approccio comprende sempre un'attività di base: prendere decisioni. Ma per prendere decisioni serve capire bene il contesto, e/o il problema per agire in maniera controllata. E spesso se non sempre, prendiamo delle misure per prendere tali decisioni. Le misurazioni in realtà stanno al centro di molti sistemi che governano le nostre vite. Basti pensare che se le misure per costruire un palazzo sono sbagliate, il palazzo crolla prima ancora di essere realizzato. Dunque le misurazioni sono pratiche comuni e necessarie a capire, controllare, e migliorare l'ambiente che ci circonda. 

Spesso però nella lingua di tutti i giorni, i concetti di misura, metrica e misurazione si sostituiscono tra loro, in maniera errata. Pertanto, definiamo formalmente tali concetti.

> [!info] Definizione di metrica
> Una metrica è una scala o un sistema di misurazione utilizzato per quantificare una misura.

La metrica quindi, definisce le regole per assegnare numeri alle misure. Nell'ingegneria del software abbiamo metriche come: il numero di linee di codice o la densità dei difetti. Trattiamo la densità e non il numero di difetti perché se con l'aumento esponenziale del numero di linee di codice il sistema software ha un densità di difetti che cresce linearmente, la qualità del software non decresce, invece se il numero di linee di codice cresce linearmente e la densità resta lineare la qualità si abbassa drasticamente. In parole povere in base al numero feature che si aggiungono ad un software, la densità di difetti ci informa quanto la sia "buono" il software. Progetti più grandi tendono a presentare un numero maggiore di difetti, mentre progetti piccoli un numero più basso. La densità fornisce una visione più chiara della qualità relativa del codice.

> [!info] Definizione di misurazione
> La misurazione è il processo mediante il quale numeri o simboli vengono assegnati agli attributi delle entità del mondo reale in modo tale da descriverli secondo regole chiaramente definite.

Da cui segue intuitivamente


> [!info] Definizione di misura
> Una misura è una rappresentazione numerica di un attributo di un prodotto software o di un processo.

Possiamo anche dire che la misura è l'effetto della misurazione, stabilita da una o più metriche.

Pertanto, la misurazione acquisisce informazioni sugli attributi delle entità. Un'entità è un oggetto o un evento nel mondo reale. Vogliamo descrivere l'entità identificando le caratteristiche che sono importanti per noi nel distinguere un'entità da un'altra. Un attributo è una caratteristica o una proprietà di un'entità. Importante è la chiarezza e non ambiguità del linguaggio, per descrivere al meglio l'entità e gli attributi. Ad esempio, se un nostro amico ci informa che pomeriggio passerà a prenderci con una Ferrari, anche non conoscendo il modello, si possono trattare delle conclusioni a monte anche senza aver mai visto tale automobile. In sostanza possiamo giudicare le entità esclusivamente conoscendo e analizzando i loro attributi.
La misurazione è essenziale per la valutazione oggettiva, il miglioramento e il processo decisionale dell'ingegneria del software. Ovviamente un misurazione può avvenire sia su una singola istanza/entità, sia su una collezione di istanze/entità. Grazie anche a quello che la fisica ci dice, **l'accuratezza di una misura dipende dallo strumento di misurazione e dalla definizione della misura stessa**.

## Scopi delle misurazioni
Le attività di ingegneria del software comprendono la gestione, la determinazione dei costi, la pianificazione, la modellazione, l'analisi, la specificazione, la progettazione, l'implementazione, il collaudo e la manutenzione. Ma a che scopi si effettuano le misurazioni?

- Valutazione: le misure del software possono essere utilizzate per valutare la qualità e l'efficacia dei processi di sviluppo del software, dei prodotti software e delle attività di manutenzione del software.
- Miglioramento: misurando vari aspetti dello sviluppo del software, è possibile identificare le aree di miglioramento e implementare modifiche che possono portare a una migliore qualità del software, produttività ed efficienza.
- Pianificazione: le misure possono essere utilizzate per stimare le risorse necessarie per i progetti di sviluppo del software, come tempo, impegno e costi. Possono anche essere utilizzate per pianificare e programmare le attività e le tappe fondamentali del progetto.
- Controllo: le misure possono essere utilizzate per verificare se i progetti di sviluppo del software stanno procedendo secondo i piani.
- Pianificazione: le misure possono essere utilizzate per stimare le risorse necessarie per i progetti di sviluppo software, quali tempo, impegno e costi. Possono anche essere utilizzate per pianificare e programmare le attività e le tappe fondamentali del progetto.
- Controllo: le misure possono essere utilizzate per monitorare e controllare lo stato di avanzamento dei progetti di sviluppo software, assicurando che siano in linea con gli obiettivi e nel rispetto del budget.
- Comunicazione: le misure possono essere utilizzate per comunicare lo stato e la qualità dei progetti di sviluppo software agli stakeholder, come i project manager, i clienti e altri membri del team di sviluppo.

A seconda dello scopo o contesto che si è elencati, occorre una conoscenza appropriata per scegliere la metrica più adeguata. In realtà se pensiamo per un momento al mondo reale, ad esempio un attaccante è valutato a seconda dei goal che fa nel corso della stagione, non delle parate! Dunque non esiste dunque una singola metrica perfetta, ma un occorre un insieme di metriche per estrarre una misurazione interessante. Tornando ai sistemi software, un disco rigido con throughput di 150MB/s per un PC è ottimo mentre per un server che ospita un database può essere estremamente lento. Insomma capiamo bene che anche le metriche che esprimono il concetto di bontà devono essere loro stesse "buone".

---



