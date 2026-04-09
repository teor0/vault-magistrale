#is2 
[[Lezione 1 - Concetto di misura, metriche e misurazione]]
# Bontà delle metriche
Come visto, non esiste una metrica perfetta che raccoglie tutte le informazioni utili in qualsiasi contesto e non solo una metrica ci può raccontare tutte le informazioni a cui siamo interessati. Ci occorre individuare delle proprietà per stabilire se una metrica è buona o meno. Insomma, andiamo definire della meta-metriche. Una buona metrica ha le seguenti proprietà: 
- Rilevanza: la metrica deve misurare un attributo significativo e importante del prodotto o del processo software. N.B. **la rilevanza varia tra dominio e dominio**.
- Obiettività: la metrica deve basarsi su dati verificabili e riproducibili, non su opinioni soggettive o stime. Dunque una metrica si suppone che porti ad una misura riproducibile.
- Consistenza: la metrica deve essere coerente con gli obiettivi e il contesto del progetto software.
- Affidabilità: la metrica deve produrre risultati coerenti in condizioni diverse e da valutatori diversi.
- Facilità d'uso: la metrica deve essere facile da comprendere, calcolare e interpretare e non richiedere uno sforzo o risorse eccessivi.

Spesso le metriche sono un compromesso tra le meta-metriche e metriche diverse suppongono decisione diverse.

Torniamo per un attimo su i difetti. All'interno del software individuiamo i difetti, bug, failure e code smell. Diamone una definizione chiara.
- Failure: è un comportamento inaspettato del sistema, fuori dai requisiti del sistema. 
- Bug: è un frammento di codice che sotto determinate condizioni porta ad una failure. Un bug regressivo è un bug creato mentre si cercava di realizzare altro.
- Difetto: è un frammento di codice che sotto determinate condizioni porta ad una failure. È l'unica definizione che incontreremo che è condivisa tra due concetti.
- Code smell: un frammento di codice che non rispetta regole di manutenibilità ma non il funzionamento del sistema.  

Per essere esaustivi, un code smell è un frammento di codice che potrebbe essere scritto meglio, ma la cui imperfezione non impatta l'utente finale, ma che impattando la manutenibilità potrebbe portare a dei difetti, i quali in futuro saranno visibili all'utente finale sotto forma di failure. Un esempio, di code smell è una parte di codice che non è raggiungibile oppure una classe con un numero di metodi spropositato, la cosi detta god class che inoltre va contro il principio di modularità dell'ingegneria del software.

Per misurare la manutenibilità del software potrei utilizzare come metrica la percentuale di bug sul numero di righe di codice oppure il numero di nuove feature sviluppate nel tempo. Altri esempi di metriche nel software sono: ![[swm.png]]
In conclusione possiamo dire che le metriche tra di loro hanno vantaggi e svantaggi.

## Scale di misurazione
Per effettuare misurazioni bisogna utilizzare scale appropriate che riflettono la natura dei dati. Per esempio nel sistema scolastico italiano, si utilizza una valutazione in decimi mentre nel sistema anglosassone si utilizza una scala letterale (A,B,C,...). Anche se il dominio d'applicazione è lo stesso, le scale utilizzate sono totalmente differenti. Andiamo a classificare le scale di misurazione in 4 tipologie:
1. Nominale: la tipologia di scala più semplice, in cui ogni entità viene assegnata ad una particolare classe, categoria, simbolo o numero in base al valore di un attributo. Non esiste dunque nessuna relazione di ordine tra le classi, e non vi è alcuna nozione di importanza/grandezza associata alle classi, simboli o numeri. Classificare un insieme di macchine in base al loro colore, presuppone l'uso di una scala nominale. Non esiste un ordine o maggiore importanza tra macchine rosse e macchine grigie. Nell'ingegneria del software, una scala nominale può essere utilizzata per rappresentare i tipi di difetti, ad esempio un difetto UI, un difetto di performance ecc.
2. Ordinale: come per la scala nominale ogni entità ha una classe associata, ma esiste il concetto di ordine. Una scala ordinale è il sistema di colori di cinture nelle arti marziali. Nell'ingegneria del software, un classico esempio è la priorità dei difetti: low, medium, high, critical. Da notare come anche se i difetti sono organizzati per priorità in maniera ordinata, la differenza tra le varie classi di priorità non è misurabile. In conclusione i numeri o classi rappresentano solo un ordine, quindi le operazioni di addizione, sottrazione e altre operazioni aritmetiche non hanno alcun significato. 
3. Intervallo: preserva l'ordine come la scala ordinale ma cattura informazioni sulla dimensione degli intervalli che separano le classi, in modo da poter comprendere la dimensione del salto da una classe all'altra, tuttavia non vi è un punto 0 nella scala. Conosciamo la differenza tra due classi qualsiasi nell'intervallo, ma calcolare il rapporto tra due classi nell'intervallo non ha senso. Ad esempio se a Roma ci sono 30°C mentre a Vienna ci sono 20°C, l'intervallo tra un grado e l'altro è lo stesso e consideriamo ogni grado come una classe correlata al calore. Ma non possiamo dire, che Roma sia il 50% più calda di Vienna. Le scale di intervallo sono utilizzate per rappresentare dati che possono essere misurati e confrontati. Nell'ingegneria del software un esempio è lo sforzo di sviluppo per intervallo di tempo, che siano ore o giorni.
4. Ratio (rapporto): la scala di rapporto è il tipo di scala più sofisticato, in cui i valori sono assegnati a categorie o classi in ordine di classificazione e l'entità delle differenze tra i valori è misurabile ed esiste un vero punto zero sulla scala. Insomma è una scala ad intervallo con la presenza di un vero punto 0 sulla scala.

Dovrebbe a questo punto risultare logico, che l'analisi è limitata/legata dal tipo di scala che si utilizza. La distanza per i valori ordinali è uguale per tutti i valori. La distanza tra cintura bianca e gialla è la stessa tra gialla e blu. Ritornando ai difetti ed alla loro severità, assegnare una categoria ad una severità non è un'attività oggettiva. Se utilizzo una scala ordinale ho meno finezza nel assegnare uno score al difetto, rispetto magari ad una scala ad intervallo da 1 a 10. Cosa io valuto 6, potrebbe essere valutato 7 un'altra persona. Mentre la scala ordinale: low, medium, high e critical, è si può restrittiva, ma permette misurazioni più omogenee tra loro. Dunque tornando al discorso delle proprietà di una buona metrica, l'obiettività è si importante ma non è facilmente raggiungibile sempre. Un altro esempio sono metriche binarie, come critical e not critical. È aumentata estremamente l'oggettività, ma è poco espressive rispetto ad altre metriche non binarie. Non esiste un silver bullet (soluzione perfetta). Infine per fare ancora più chiarezza sull'importanza dell'interpretazione delle misurazioni, introduciamo il concetto di misurazione significativa:

> [!info] Misurazione significativa
> Diciamo che un'affermazione che coinvolge una misurazione è significativa, se il suo valore di verità è invariante rispetto alle trasformazioni delle scale consentite.

Una trasformazione è ammissibile se preso un mapping, si passa da una misura accettabile ad un'altra. Ad esempio per la temperatura una trasformazione ammissibile è quella da Celsius a Kelvin e viceversa. Lo statement "la temperatura oggi a Roma è il doppio che a Berlino" non è significativo, in quanto la verità o falsità dello statement non rimane consistente rispetto al misura che utilizziamo, che sia essa Celsius o Kelvin. In conclusione, la scelta della scala utilizzata nella misurazione e nell'analisi del software, dipende dalla natura dei dati misurati e dall'uso previsto dei risultati. Comprendendo i diversi tipi di scala, possiamo garantire che le nostre misurazioni siano appropriate, accurate e significative, portando a un processo decisionale migliore ed a una qualità del software migliorata.

---


