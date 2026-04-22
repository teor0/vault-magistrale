#pmcsn #magistrale 
# Performance evaluation e definizione di modello
Tratteremo da modellazione, simulazione e analisi di sistemi. Il sistema di riferimento è quello delle rete di code, sia solo singola che multipla. Perché analizzare le prestazioni delle reti di code? Perché sempre più spesso i sistemi distribuiti vanno down per picchi di richieste. Un buon modello di valutazione delle prestazioni fornisce una profonde comprensione del comportamento del sistema, ovvero perché il sistema si comporta come si comporta, cosa limita il comportamento del sistema e quali problemi devono essere risolti per poter migliorare il sistema.

Elenchiamo alcuni degli obiettivi della performance evaluation:
- Capacity planning: determinare il numero e la dimensione delle componenti del sistema.
- System tuning: determinare il valore ottimo dei parametri del sistema.
- Bottleneck identification: determinare il collo di bottiglia delle performance.
- Workload characterization: caratterizzare il carico di lavoro del sistema.
- Forecasting: predirre le performance del sistema al crescere del carico di lavoro.

In particolare il workload characterization nell'ingegneria del software è tipica delle comunicazione via internet, e diciamo si comporta in maniera totalmente diversa rispetto a sistemi basati su studi "classici". All'interno del corso tuttavia ci limiteremo ai primi 3 obiettivi della performance evaluation. Una cosa che deve essere chiara subito è che dagli obiettivi che mi pongo, costruisco un modello. Ad esempio per un service provider non basta sapere qual è il tempo di risposta medio, ma vuole sapere con che probabilità il tempo di risposta medio superi una soglia. Questo perché magari il provider ha un SLA sul tempo di risposta media con annessa penale.

La performance evaluation è sicuramente utile nella ricerca per dimostrare la validità di una nuova idea. Nell'industria o nei servizi, si usa per cerca di mantenere un alto livello di performance durante la vita del sistema. Nella costruzione del modello non ci sono linee guida, ciò rende difficile la fase di costruzione, ma come detto gli obiettivi tracciano la strada. Prima di costruire un modello, si inizia con una comprensione approfondita del sistema, dell'applicazione e degli obiettivi dello studio secondo due approcci:
1. Valutazione sperimentale sul sistema vero: sempre valido, un esperimento racchiude nozioni accurate sul comportamento del sistema sotto determinate ipotesi. 
2. Intuito e estrapolazione dei trend: necessita molta esperienza e conoscenza del dominio per produrre un interpretazione generalizzata di un modello di un sistema in astratto.

Nel primo caso si ha un sistema vero "acceso" mentre il secondo un sistema "spento". L'approccio sperimentale è costo, preciso ma poco flessibile mentre l'approccio intuitivo è flessibile e poco costoso dato che è basato su basi teoriche, ma poco preciso. La modellazione sta nel mezzo tra i due approcci. 


> [!info] Definizione modello
> Un modello è un'astrazione di un sistema: un tentativo di distillare, dalla massa di dettagli che costituisce il sistema stesso, proprio quegli aspetti che sono essenziali per il comportamento del sistema.

Definito attraverso un processo d'astrazione, parametrizzato per riflettere qualunque alternativa sotto studio e valutato per determinare il comportamento del sistema. In conclusione la modellazione è meno dispendiosa e più flessibile di una valutazione sperimentale e più affidabile di un'intuizione nata dall'esperienza. La modellazione sfrutta tecniche computazionali e matematiche per la performance evaluation quali, misurazione, analisi tecnica e simulazione per produrre conclusioni dall'output ricavato. Più che modelli analitici, essendo ingegneri informatici, tratteremo i modelli simulativi, in cui dopo aver l'implementato un modello ci baseremo sul risultato della simulazione per trarre conclusioni.

---