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
