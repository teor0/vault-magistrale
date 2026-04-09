#is2 #magistrale 
[[Testing 1 - Verifica e validazione]]
dall'idea di correttezza passiamo all'affidabilità (reliability)
non è detto che un sistema con alta reliability sia migliore di un sistema con reliability bassa, questo perché la reliability è una stima che dipende dal modo in cui osservo il sistema. insomma non è una metrica oggettiva.
definiamo come reliability 1-PFD (probabilità di fallire). la reliability è una stima se PFD è una probabilità mentre è un valore concreto se PFD è il conteggio di un report.
il fallimento è osservabile a differenza del successo.
profilo operazionale: si intende una descrizione numerica di come un sistema è utilizzato.
differenti profili di descrizione in funzione delle tipologie di utenti.

i processi di quality assurance sono attività sistematiche che forniscono prova di fitness per l'uso del prodotto software totale.

quando parliamo di software quality assurance identifichiamo un insieme di piani ed attività per il monitoraggio e controllo del processo di sviluppo software.

nel processo di CI aggiungo feature in maniera continuativa
i sistemi di CI replicano il processo di build in ambienti diversi da quello di sviluppo. quali e quanti ambienti diversi da quello di sviluppo è una prerogativa del team di quality assurance. 
nel CT ad ogni nuova aggiunta effettuo dei test per verificare se i test sviluppati finora siano o meno violati
un processo di quality assurance è regolato da diversi elementi
il software testing è solo uno degli aspetti del quality assurance
vogliamo che il processo di testing sia il più vicino possibile al processo di controllo della qualità

---
# JUnit
le esecuzioni dei test non hanno un ordine deterministico.
durante la fase test, maven si occupa di invocare il motore JUnit, ma non può leggere le annotazioni di test. dunque a meno di configurazioni diverse, il nome della classe deve seguire uno tra i seguenti patter:
- Test*
- \*Test
- \*Tests
- \*TestCase
  
tra unit e integration testing, la differenza sta nell'obiettivo che mi pongo, non il mezzo per arrivarci. nel unit testing, ogni modulo in autonomia deve comportarsi in maniera corretta. nel integration testing, si mettono insieme istanze di moduli che hanno superato lo unit testing per verificare che l'insieme si comporta in modo corretto. 
nel report del progetto occorre motivare i risultati ottenuti secondo approccio black o white box.
esistono diversi tipi di test
![[tipi_test.png]]
a seconda dell'obiettivo si sceglie il tipo di test. nota: i test di level acceptance sono gli unici in cui non è richiesto una conoscenza dell'informatica, è più un test a livello consumer. anche la densità per tipologia di test è diversa: ![[disp_test.png]]

v-model: in sostanza ad ogni processo di sviluppo, si ha un processo speculare nell'ambito del testing. per la concept of operations gli acceptance test, per i system requirements i system test ecc.

il progettista dei test, dovrebbe utilizzare le specifiche del sistema per progettare i test. ad esempio per una classe singleton, se un test di creazione di più istanze ha successo, allora la progettazione ha un problema. di sicuro non si effettua un test che non ha senso per il pattern singleton.

il flusso può essere schematizzato come segue:
1. il progettista produce la specifica ed l'implementazione delle funzionalità
2. avviene il rilascio delle specifiche ai tester
3. il progettista dei test realizza la specifica e l'implementazione dei test

per gli unit test l'obiettivo è rivelare malfunzionamenti su un singolo modulo/unità funzionale
chi sviluppa il modulo/unità sarà anche chi progetterà i test avendo come riferimento l'implementazione (white-box) o la funzionalità astratta (black-box). gli unit test possono essere anche scritti prima dell'implementazione della funzionalità, andando a perseguire il test driven development (TDD).
quando smettere di testare? ci sono diversi criteri in base al numeri di funzionalità controllate, requisiti controllati ecc. oppure in base alla copertura del codice.

per JUnit4 i parametri dei costruttori non di default, si trovano all'interno di un metodo statico annotato con @Parameters
senza @RunWith JUnit usa il runner di default che si aspetta che la classe di test abbia un costruttore di default pubblico.
vedi documentazione runner per maggiori dettagli.
anche i metodi annotati con @BeforeClass o @AfterClass devono essere statici.
@After avviene dopo l'esecuzione di un singolo test, non dopo l'esecuzione di tutti i test.

c'è differenza tra sbagliare la progettazione dei test e l'implementazione.
se si progettano i test solo tenendo conto degli input e senza anche gli output si fa un lavoro a metà. 
quando si progetta un test bisogna avere un input ed un output atteso.

per gli integration test l'obiettivo è rilevare i malfunzionamenti derivati dall'interazione di due o più moduli. chi definisce tali test dipende dagli scenari di integrazione considerati. anche in questo caso è possibile basarsi sull'implementazione (white-box) oppure sulla funzionalità (black-box). si necessità di tecniche si stub/mock/emulazione delle funzionalità che non voglio testare.


vedremo come collegare debug del IDE a Maven.
