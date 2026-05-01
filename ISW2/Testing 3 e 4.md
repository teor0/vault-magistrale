#is2 #magistrale 
17/04
nel mocking di test di unità soffro di meno il mocking rispetto al testing d'integrazione. 
l'obiettivo della verify è avere sempre la stessa istanza di dao.
inizio lezione 3°bookmark
parliamo di approccio alla generazione di test, in particolare dato un dominio di input, andiamo a scegliere un sottoinsieme di input il più rappresentativo possibile di punti in cui vi sono failure. gli approcci non sono classificati in base all'efficacia ma in base al caso d'uso.
il primo approccio è l'uso di dati di esecuzioni storiche. può tornare utile utilizzare tracce passate come input. uno dei vantaggi è l'utilità di tali dati per attività di validazione su i requisiti del sistema.
generare dei test vuol dire dato un input predirre un output nel caso di generazione di test guidata dall'adequacy vedi 4° bookmark
scegliere i valori giusti non sempre è indice di qualità.
nella fase di manual testing utilizzeremo la tecnica di partizione del dominio di input.

è consigliato leggere l'articolo di randoop

---
22/04
non confondere i parametri del test con i parametri formali di un metodo o di una classe.
più i testi sono legati all'implementazione più i testi sono legati al contesto a contorno. mentre se il test si basa sulla semantica ci si stacca dall'implementazione e si testa la funzionalità.
per le strutture con tipi di dati diverso devo iterativamente fare un analisi con le altre linee guida.

le classi di equivalenza le scelgo in base a concetti diversi come, la conoscenza del dominio o l'esperienza soggettiva

non confondere il concetto di validità di un parametro con il concetto di correttezza legato ad una specifica configurazione del SUT. L'esempio che vediamo è quello di una mail valida sintatticamente, ma che può essere corretta o meno se esiste lo User legato a tale mail.

nel caso unidimensionale: per ogni classe di equivalenza di un parametro esiste un test
nel caso multidimensionale: effettuo il prodotto cartesiano 

LedgerHandle è un parametro del test o meglio il contesto. Mi aspetto comportamento diverso a seconda del suo valore perciò lo considero nelle classi d'equivalenza.
Partizioni significative di LedgerHandle da consegnare in mail entro martedì.

Esempio di invalid istance è una classe test (mock) che con realizza un costruttore che lancia un'eccezione o comunque che sia invalida.

Fatta la classe d'equivalenza, tutti gli elementi appartenenti ad essa hanno lo stesso valore, appunto, equivalenza. 

Seppur non esiste un teorema che prova ciò, i valori a confine delle classi di equivalenza portano ad errori.

---
29/04

continuiamo sull'analisi de le classi di equivalenza su i valori al confine.
2° bookmark 24:00

nella combinazione unidimensionale per ogni boundary value è necessario avere una tupla di test in cui occorre il boundary value.
le combinazioni non vanno a priori escluse. le scelte sulle combinazioni vanno documentate. 36:30

slide 52 utile per progetto. noi sperimenteremo il manual software testing.
pure prompting è un interazione diretta tra umano e LLM, senza strumenti aggiuntivi nel mezzo.
al output del LLM si dovrebbe affiancare un meccanismo di validazione.
noi utilizzeremo il pure-prompting schema con diverse varianti come lo zero shot o few shot prompting.
vedi proposta esercizio.
01:30:00 recap per il progetto.
