#is2 
[[Lezione 2 - Bontà delle metriche e scale]]
# Standard ISO 25010, Object-oriented metrics e CK metrics
Lo standard di sicurezza ISO 25010 fornisce una tassonomia di attributi di qualità, ma non fornisce un programma di misurazione per misurare tali qualità. Nella pratica è spesso utilizzato nella specifica dei requisiti, il QA planning e il benchmarking. Nello standard il modello definisce che cosa si intende per qualità, mentre le metriche definiscono il come quantificarle. Le caratteristiche di qualità possono essere suddivise in base all'obiettivo che si vuole raggiungere come la sicurezza, l'efficienza nelle performance, la manutenibilità o la sostenibilità funzionale. Lo standard permette di beneficiare di un vocabolario standardizzato che riduce l'ambiguità nei requisiti di qualità e nei trade-off. Abilità la tracciabilità a partire dalle richieste degli stakeholder, agli attributi di qualità fino ad un criterio misurabile di accettazione.
Gli standard di sicurezza sono una scala ordinale. risentire

Le object-oriented metrics (OO) vengono utilizzate per misurare la qualità di sistemi software orientati agli oggetti. Aiutano gli sviluppatori ad identificare difetti di design, problemi di performance e manutenibilità, presto nel ciclo di sviluppo. Esistono diverse metriche OO che possono essere utilizzate nei sistemi software, ma in realtà le metriche OO sono indicatori di rischio e la loro interpretazione va utilizzata per confronti, come se fossero trend e mai come valori assoluti, preferendo le distribuzioni rispetto a medie globali. Dunque è importante comparare due sistemi in base alle metriche. Un tipo particolare di metriche object-oriented sono le metriche CK, che consistono in sei differenti metriche che possono venire utilizzate per misurare vari aspetti del design di sistemi object-oriented, come l'ereditarietà, l'accoppiamento e la complessità. Di particolare importanza è il concetto di complessità intrinseca. La complessità è qualcosa che monitoriamo affinché non esploda all'interno del sistema. Un componente molto complesso, porta problematiche di manutenibilità e performance. La complessità del codice è cruciale per il progetto: 1 milione di righe di codice sono complesse da gestire, ma se non vengono manutenute il progetto rischia di diventare obsoleto.


> [!important] Complessità intrinseca
>  La complessità intrinseca è la complessità che per natura caratterizza un problema. Non sempre lato software, qualcosa di complicato se reso meno complicato è più semplice da gestire. 

Tornando alle metriche CK le sei differenti metriche sono:
![[CK_metrics.png]]
Oltre alle metriche CK esistono altre metriche object-oriented che non sono relegate solo alle classi, ma l'utilizzo può essere applicato a livello di metodi, classi, package o processo. La complessità ciclomatica a livello di metodo, misura la complessità di un programma contando il numero di path del flusso di controllo indipendenti nel codice. Può essere calcolata tramite l'analisi statica del codice e un alto valore di complessità ciclomatica, è caratteristico di software che sono più difficili da comprendere e mantenere. Il suo utilizzo è utile nel testing e nel calcolo del rischio di manutenibilità, ricordando che le soglie dipendono dal contesto e di preferire trend e valori outliner.
Ci sono poi il Fan-In e Fan-Out, dove il Fan-In misura il numero di classi che dipendono da una data classe, dunque un valore elevato indica che quella classe è critica all'interno del sistema. Il Fan-Out invece misura il numero di classi da cui dipende una data classe, un alto valore indica una classe che è troppo dipendente e dunque necessità di un refactoring e riduzione delle responsabilità.

---
# Function points 
Partiamo dal codice per introdurre i function points, un approccio semplice e ripetibile. I function points, sono una tecnica di misurazione della dimensione del software che quantifica la funzionalità fornita da un'applicazione software. La metrica sono i punti funzione indipendenti dal linguaggio di programmazione, la tecnologia ed i dettagli di implementazione. Si basano sulla prospettiva degli utenti ed il valore di business dell'applicazione software.
La metrica può essere misurata prima che il progetto sia implementato, ciò permette di stimare il costo e di pianificare lo sviluppo, una volta che ho requisiti funzionali del sistema o una descrizione della funzionalità del sistema. L'essere completamente indipendente dai requisiti di qualità e dai concetti tecnologici suggerisce che questa metrica, da una visione di insieme e non dettagliata. I fattori non funzionali infatti, impattano molto di più sul costo più dei fattori funzionali, basti pensare che sviluppare una nuova feature del sistema, costa meno che migliorare una già presente. All'inizio della progettazione del sistema software è utile guardare aspetti funzionali. 

I function point si basano su diversi fattori quali:
![[fp.png]]
dopo aver identificato i function point, ne classifichiamo la complessità e andiamo a calcolare la somma applicando i pesi. ![[fp_cost_tab.png]] Alla fine otteniamo una stima di quanto il sistema sia grande dal punto di vista funzionale, ovvero una stima della grandezza di quanto "lavoro" sia fa il sistema sia necessità la realizzazione del sistema. La trasformazione da nominale a numerico permette di fare somme e medie, tuttavia questo passaggio non è obiettivo ma artificioso. Ad esempio 4 medaglie d'argento non fanno una medaglia d'oro, vedasi conteggio medagliere olimpico.
I function points sono molto utili per la PROGETTAZIONE e la fase di produzione del sistema, non nella fase di ciclo di vita e manutenzione. Ricordiamo che, come tutte le metriche i function points si utilizzano per i confronti e prendere decisioni.
Limitazioni:
- Assenza del concetto di qualità.
- Non cattura l'intero intervallo di funzionalità software ed attributi di qualità come l'usabilità, la manutenibilità e l'affidabilità.
- Sono soggetti ad interpretazioni e giudizi soggettivi, dato che i pesi e fattori di conversione possono variare a seconda degli stakeholders e dei contesti. 
- Non sono applicabili per tutti i tipi di applicazioni software, come ad esempio quelli con un forte enfasi sull'interfaccia utente o il real-time processing.

---
# GQM+S
L'approccio GQM+S, nasce dall'integrazione di strategie, volte ad identificare e selezionare la strategia più appropriata per raggiungere gli obiettivi, alla metodologia Goal-Question-Metric (GQM) in cui si definiscono appunto obiettivi, domande e metriche. GQM+S collega gli obiettivi di business, alle strategie agli obiettivi di misurazione, per assicurarsi che le metriche ci indirizzano su decisioni organizzative. ![[gqm.png]]
GQM+S sta all'interno di un "meta-algoritmo", organizzato come un processo ciclico in cui GQM+S stesso è un ciclo.
Alcuni usi tipici sono:
![[gqms-uses.png]]
In particolare la strategia descrive quali azioni organizzative tecniche debbano essere prese. L'obiettivo specifica come il successo sarà giudicato. La metrica rende operativa la valutazione. Le domande garantiscono l'allineamento tra gli obiettivi e le evidenze. Vediamo alcuni esempi:
![[gqm_examples.png]]
il code coverage è una metrica che rappresenta quanto il test copre l'applicazione e spesso viene misurata a livello di line di codice. Una code coverage del 90% significa che i test eseguono il 90% dell'applicazione.

---
# Stima dei costi del software e planning poker
All'interno dello sviluppo di sistemi software, come in altri ambiti è importa la stima dei costi, intesa come costi monetari, tempo necessario per lo sviluppo, sforzo necessario per completare un'attività, ecc. Nei costi dei componenti software, esistono i costi di hardware e license software, costi di training degli sviluppatori, ma soprattutto nella gran parte dei progetti il costo dello sforzo, inteso come lo stipendio degli sviluppatori.

Le stime sono fatte per scoprire i costi allo sviluppatore, nel produrre un sistema software, tuttavia la relazione tra costi di sviluppo e prezzo al committente, non è immediata nel software. Diverse considerazioni di tipo organizzativo, economico e politico influenzano il cambiamento del prezzo. Un esempio può essere la scelta del linguaggio di programmazione. Se scelgo C come linguaggio rispetto a Rust, magari il tempo di sviluppo sarà minore. Esistono diverse tecniche per stimare i costi quali:
1. Modello algoritmico dei costi: Un approccio stereotipato basato sul costo storico delle informazioni e che è generalmente basato su dimensione del software
2. Giudizio dell'esperto: uno o più esperti nello sviluppo di software ed il dominio dell'applicazione, usano l'esperienza per predirre i costi. Vantaggio: la stima può essere fatta senza costi aggiunti. Svantaggio: non sapere l'esperienza dell'esperto. Inoltre il non avere costi nella duplicazione dei sistemi software, porta ad avere sempre sistemi diversi per domini applicativi uguali.
3. Stima per analogie: il costo del progetto si basa sul confrontare il progetto ad altri progetti simili nello stesso dominio applicativo. Vantaggio: la stima è accurata se i dati dei progetti sono disponibili. Svantaggio: senza confronto non si può fare alcuna stima.
4. Legge di Parkinson: il progetto costa qualunque siano le risorse disponibili. La gestione del tempo è tutto psicologico. Ci avviciniamo naturalmente al ritmo per finire un progetto appena in tempo. Lo stesso compito può richiedere un'ora o una settimana a seconda di come quanto tempo ci concediamo per completarlo. Vantaggio: non si spende oltre il budget. Svantaggio: il sistema spesso è incompleto.
5. Pricing to win: il progetto costa quanto è il budget fornito dal committente. Vantaggio: si ha un contratto. Svantaggio: la probabilità che il committente riceva il sistema che ha chiesto è bassa, perché i costi non corrispondono al lavoro necessario per lo sviluppo.

Il planning poker è una tecnica di stima sfruttata nella metodologia agile, grazie alla sua semplicità ed accuratezza. Definiamo le regole del planning poker:
1. Ogni partecipante riceve un mazzo di carte che rappresentano le stime sotto forma di sequenza di numeri, in cui sono presenti dei vuoti. I mazzi più popolari sono la sequenza di Fibonacci ed i numeri che raddoppiano. I vuoti nel mazzo, servono per pensare con granularità differente.
2. Il moderatore presenta una user story alla volta al team.
3. Il product owner o chi per lui, risponde a qualsiasi domanda che il team ha sulla user story.
4. Ogni partecipante seleziona privatamente una carta che rappresenta la sua stima della "dimensione" della user story. Dove per dimensione si intende il tempo necessario, il rischio, la complessità ed altri fattori rilevanti della story.
5. Quando tutti hanno una stima, le carte vengono presentate e si dibatte. Spesso si usa un timer per il dibattito.
6. Se si raggiunge il consenso su una stima, il valore viene salvato ed il team passa alla prossima story. In caso di pareggio si prende il valore più alto, per avere un margine.
7. Nel caso le stime differiscono, il valore più alto e più basso di stime, dibattono con tutte le altre.
8. Un nuovo round di valutazione avviene. Passo 4.
9. Si continua finché non si raggiunge il consenso.
10. Si itera su tutte le storie per raggiungere il consenso.

Ci sono vari motivi per cui il planning poker viene usato: 
- promuove la collaborazione tra i membri del team.
- si crea consenso nel scegliere la stima, invece che avere una singola persona che si occupa di scegliere la stima. 
- Espone i problemi presto attraverso la discussione delle user story.

Si consiglia di avere un membro del team che crea le user story assieme al QA e lo sviluppatore capo, prima che avvenga il planning poker, cosi che il resto del team si possa concentrare sulla stima della user story. Da notare, che chi partecipa al planning poker, ovvero gli sviluppatori e non i manager, è chi lavora sulla user story, chi non vota non lavora su di essa. In un certo senso il voto dà un senso di responsabilità comune. Inoltre il costo stimato non ha una metrica come soldi o tempo, è una metrica a sé come per i function points e l'uso del costo è sempre comparativo. 

---