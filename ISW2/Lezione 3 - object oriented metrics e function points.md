#is2 
[[Lezione 2 - bontà delle metriche e scale]]


Gli standard di sicurezza sono una scala ordinale.
Le OO metrics vanno utilizzate come trend e mai come valori assoluti. 
è importante comparare due sistemi in base alle metriche. Le CK metrics sono metriche OO. 
Concetto di complessità intrinseca: il concetto di complessità è qual cosa che monitoriamo affinché non esploda all'interno del sistema. un componente molto complesso, porta problematiche di manutenibilità. la complessità del codice è cruciale per il progetto. 1 milione di righe di codice sono complesse da gestire, ma se non vengono manutenute il progetto rischia di diventare obsoleto. 
la complessità intrinseca è la complessità che per natura caratterizza un problema. (rivedi definizione)
fan-in e fan-out: quanto una classi viene usata e quanto utilizza altre classi.
Le metriche object-oriented non sono relegate solo alle classi, l'utilizzo può essere applicato a diversi livelli:
- metodi
- classi
- package
- processo

nella CC il path è quello del flusso di controllo.
partiamo dal codice per introdurre i function points, una tecnica di misurazione della dimensione del software che quantifica la funzionalità fornita da un'applicazione software. la metrica sono i punti funzione indipendenti dal linguaggio

la metrica può essere misurata prima che il progetto sia implementato, questo permette di stimare il costo una volta che ho requisiti funzionali del sistema. i fattori non funzionali però impattano sul costo più dei fattori funzionali.
all'inizio è utile guardare aspetti funzionali.
gli fp sono un approccio semplice e ripetibile.
alla fine otteniamo una stima di quanto il sistema sia grande dal punto di vista funzionale. mostra insomma una stima della grandezza di quanto "lavoro" sia fa il sistema sia necessità la realizzazione del sistema.
la trasformazione da nominale a numerico permette di fare somme e medie. tuttavia questo passaggio non è obiettivo ma artificioso. 4 medaglie d'argento non fanno una medaglia d'oro vedasi conteggio medagliere olimpico.
gli FP sono molto utili per la PROGETTAZIONE e la fase di produzione del sistema, non nella fase di ciclo di vita e manutenzione.
come tutte le metriche i FP si utilizzano per i confronti e prendere decisioni

approccio GQM+S. nasce dall'integrazione di strategie alla metodologia GQM
GQM+S sta all'interno di un "meta-algoritmo", organizzato come un processo ciclico.
code coverage metrica che rappresenta quanto il test compre l'applicazione e spesso viene misurata a livello di line di codice, una cg del 90% significa che i test eseguono il 90% dell'applicazione.

value testing: passo da un tempo d'esecuzione medio di 10s a 9.5s
strategy aumentare lo user satisfactiong rating del 10% riducendo il numero di click di un 1/3
esercizio riempire le altre caselle.
caso improve sw dev process numero di ticket per iterazione

---
la relazione tra costo e prezzo non è immediata nel software

svantaggio reale expert judgement: non sapere l'esperienza dell'esperto. l'altro lato della medaglia, è che il non avere costi nella duplicazione dei sistemi software, porta ad avere sempre sistemi diversi per domini applicativi uguali.
planning poker nasce con i metodi agile. il costo non ha una metrica quali soldi o tempo, è una metrica a sé come per i fp.
l'uso del costo è sempre comparativo.
le lacune nel mazzo, servono per pensare con granularità differente.
il voto da anche un senso di responsabilità comune.
