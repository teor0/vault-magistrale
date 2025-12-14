#amw  #magistrale 
[[Definizioni]]
# Definizione reverse engineering
Prima diamo la definizione di reversing attività presente da molto prima dell'informatica.
> [!info] Definizione di reversing
> Il processo di estrazione della conoscenza e degli schemi di progetto di qualsiasi oggetto costruito dall’uomo.

La ricerca scientifica e il Reverse Engineering sono per certi aspetti molto simili tra loro: estraggono da un insieme di dati quantitativi, producono un modello formale che rappresenti in modo adeguato la “realtà”, che non è direttamente desumibile dalla base di dati raccolti. Ma bensì utilizzando l’intuizione personale e utilizzando opportune metodologie che consentono la continua verifica dei risultati ottenuti. All'interno del corso ci occuperemo del reverse code engineering (RCE):

> [!info] Definizione reverce code engineering
> Il processo di ricostruzione delle finalità, delle strutture di dati, dell’algoritmo e dei dettagli implementativi di un programma per calcolatore elettronico

L’input del RCE è una rappresentazione “a basso livello” di un programma per calcolatore generalmente il file eseguibile contenente il codice macchina. L’output del RCE è una rappresentazione “ad alto livello” delle conoscenze apprese durante l’attività di reversing ad esempio, la descrizione degli effetti del programma.

> [!warning] Da non confondere
> “Alto livello” e “basso livello” possono essere concetti relativi! Un sorgente scritto in c potrebbe essere illeggibile mentre il suo disassemblato no

I motivi per i quali si intraprende l’attività di RCE possono essere svariati e molteplici:
- produrre adeguata documentazione del funzionamento di un proprio programma sviluppato in passato
- sviluppare un programma che si ponga come concorrente di un altro programma di terze parti
- verificare che il funzionamento di un programma di terze parti non costituisca un rischio per la propria organizzazione
- superare i meccanismi di protezione di materiale digitale (film, musica, libri, giochi elettronici, software)
- analizzare il comportamento del malware al fine di determinare i suoi effetti, sviluppare un sistema di protezione o derivarne una nuova versione

Alcune attività di RCE sono illegali in quasi tutte le nazioni del mondo, altre sono legali o tollerate. La maggior parte delle applicazioni commerciali vengono utilizzate con licenze d’uso che esplicitamente proibiscono l’attività di RCE. Spesso una attività di RCE è in una zona grigia, ad esempio, in Italia sembra essere consentito fare RCE del software di una stampante allo scopo di vendere cartucce d’inchiostro riciclate, ma allo scopo di produrre cartucce compatibili è illegale (HP? sei tu?).

## Principi del RCE
Un buon programmatore adotta una serie di buoni principi di programmazione che facilitano lo sviluppo di software di qualità. Analogamente, il bravo hacker cerca di ottemperare ad una serie di principi di RCE che facilitano ed accelerano il suo lavoro.
Elenchiamo di seguito alcuni principi cardine per la RCE.


> [!important] Principio zero
> Fare copie del malware una volta entrati in suo possesso e MAI eseguire il malware al di fuori di una macchina virtuale.

> [!important] Primo principio
> Maggiore è la comprensione dell’intero sistema, tanto più rapida, efficiente e produttiva è l’attività di reversing

Chi fa reversing deve avere conoscenze consolidate in vari ambiti come l'architettura dei calcolatori elettronici, sistemi operativi, processo di compilazione dei programmi, ecc. Maggiore è la conoscenza dell'analista, più velocemente sarà il processo di reversing.
> [!important] Secondo principio
> Per capire il codice scritto da qualcun altro, è necessario capire come funziona il proprio codice

Il programma in esame è il risultato di un processo di compilazione di un linguaggio ad alto livello scritto da uno o più programmatori. L'obiettivo del RCE non è la comprensione di ogni singola istruzione macchina ma la comprensione del funzionamento del programma.


> [!important] Terzo principio
> La qualità e la conoscenza degli strumenti di RCE determina la qualità dell’intero processo di reversing

Possedere e padroneggiare appropriati programmi di analisi semi-automatica del codice e del comportamento dei programmi è essenziale per la rapidità del RCE.

> [!important] Quarto principio
> La chiave del reversing è l'abilità di identificare e comprendere gli schemi ricorrenti nel codice; di conseguenza, non esiste sostituto dell’esperienza!

L'abilità principale dell'hacker esperto è quella di saper riconoscere gli schemi nascosti nel codice macchina, dato che risulta difficile scrivere i programmi di decompilazione automatica che producono un sorgente ad alto livello facile da comprendere, o comunque gli schemi meno diffusi sono difficilmente comprensibili dal disassemblatore

## Metodologie
È importante adottare una precisa metodologia nell'attività di reversing meglio se formalizzata con la redazione di documentazione scritta, anche perché nell'informatica le cose cambiano rapidamente. Questo perché quando si intraprende qualunque attività di RCE, è necessario avere ben chiari gli obiettivi che ci si prefigge,  in secondo luogo stabilire in che modo operare per ottenere gli obiettivi ed infine verificare continuamente la correttezza delle informazioni ottenute. In generale si hanno tre macrocategorie di metodologie per l'analisi del malware. Il primo metodo è l'analisi <font color=orange>black box</font> o live analysis, si pone di osservare il funzionamento del programma "dal vero", ossia senza esaminare la sua struttura interna. Ciò presuppone l'utilizzo di una VM, per isolare il potenziale distruttivo del malware. Talvolta però questo approccio è rischioso o impossibile da effettuare dati i costi, temporali e/o economici.  Questa metodologia non può fornire informazioni su porzioni di codice non attivate dall’esecuzione. La seconda metodologia è l'analisi <font color=orange>white box</font> o dead listing analysis, che analizza il codice macchina codificato nel file eseguibile del programma in esame attraverso disassemblatori e i decompilatori, risultando anch'essa dispendiosa in termini di impegno e tempo richiesto. Infine l'ultima metodologia è l'analisi <font color=orange>grey box</font> o mixed analysis, che appunto combina l'approccio black e white box, andando a osservare il funzionamento del programma eseguendo il suo codice macchina in
modo controllato, attraverso strumenti quali: debugger e applicativi di sistema. Metodologia in genere molto efficace, anche se esistono tecniche ad-hoc per ostacolarla. Proponiamo di seguito un esempio di metodologia che ovviamente non comporta un analisi rigida e iterativa senza passi indietro.
1. Descrizione preliminare: consiste nel descrivere ciò che si conosce del programma prima di iniziare il reversing come: la provenienza, ovvero il contesto dell'incidente, luogo e data dell'incidente ecc. L'ipotesi sulle finalità, circostanze in cui è attivato, file prodotti o modificati ecc.
2. Formalizzazione dell’obiettivo: consiste nel formalizzare la finalità dell’attività di reversing come identificare il meccanismo di protezione del programma oppure ricostruire un algoritmo utilizzato per una certa funzionalità. Si va così a ridurre i rischi di perdere tempo.
3. Ottenimento del codice macchina: può essere immediato e banale dato che il programma potrebbe essere cifrato. Comunque è possibile fare RCE sul codice cifrato.
4. Osservazione del funzionamento: se possibile, si dovrebbe cercare di ottenere ogni possibile informazione osservando il funzionamento del programma come ad esempio quali chiamate di sistema vengono utilizzate. In alcuni casi ciò non si riesce a fare per mancanza della piattaforma hardware o del codice completo.
5. Disassemblaggio del codice macchina: è necessario disassemblare il codice macchina anche se è possibile che non riesca a decodificare correttamente. 
6. Localizzazione di un frammento assembly: Se il codice macchina è lungo, è necessario identificare una porzione di codice
che sia interessante per perseguire l’obiettivo del reversing, come ad esempio funzioni di libreria o system call.
7. Analisi del frammento assembly: Si analizza un frammento di programma in assembly per arrivare a comprendere soprattutto le strutture di dati utilizzate, il resto viene in cascata.
8. Verifica dei risultati: quando si ritiene di aver raggiunto l'obiettivo del reversing, è essenziale cercare di verificare la correttezza delle conclusioni raggiunte con una procedura possibilmente differente da quella impiegata per il reversing, come ad esempio creare una patch che disattivi il meccanismo di protezione.
9. Riepilogo delle informazioni ottenute: attraverso la realizzazione di un documento scritto contenente tutto il processo di reversing, i risultati ottenuti e informazioni collaterali apprese durante il processo.

---