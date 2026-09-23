#magistrale #pmcsn 
[[16 - Generatori multi stream]]
# Definizione di next event simulation
Con il crescere della complessità a livello concettuale, con la event driven simulation la complessità a livello computazionale cresce esponenzialmente. Serve dunque un approccio più semplice e generale. Tale approccio è la **next-event simulation**  che si basa su alcune importanti definizioni e terminologie: 
1. Stato del sistema 
2. Eventi
3. Clock della simulazione
4. Scheduling degli eventi 
5. Lista degli eventi
   
> [!important] Definizione stato del sistema
> Lo stato di un sistema è una caratterizzazione completa di un'istanza del sistema nel tempo: uno "snapshot" completo nel tempo. Nella misura in cui lo stato di un sistema può essere caratterizzato assegnando valori alle variabili. Quindi le variabili di stato sono utilizzate per questo scopo.

Per costruire un modello di simulazione ad eventi discreti utilizzando l'approccio next event, il focus consiste nell'affinare la descrizione dello stato del sistema e della sua evoluzione nel tempo. Al livello del modello concettuale lo stato di un sistema esiste solo in astratto come insieme di possibili risposte alle seguenti domande: cosa sono le variabili di stato, come sono correlate e come si evolvono nel tempo? A livello di specifica lo stato del sistema esiste come una raccolta di variabili matematiche, le variabili di stato, insieme a
equazioni e logica che descrivono come le variabili di stato sono correlate e un algoritmo per calcolare la loro interazione ed evoluzione nel tempo. A livello computazionale lo stato del sistema esiste come una raccolta di variabili di programma che caratterizzano collettivamente il sistema e sono sistematicamente aggiornate con l'evoluzione del tempo simulato. 

>Un modo naturale per descrivere lo stato del centro a servente singolo è utilizzare il numero di job nel centro come una variabile di stato.


> [!important] Definizione evento
> Un evento è un occorrenza che **può** modificare lo stato del sistema. Per definizione, lo stato del sistema può cambiare solo al momento di un evento. Ogni evento ha un tipo associato.

Per un centro a servente singolo con o senza feedback esistono solo due tipi d'evento: arrivo di un job e completamente del servizio di un job. L'arrivo aumenterà sempre il numero di job nel centro di uno. Per centro senza feedback, il completamento di un job decrementerà sempre il numero di job di uno, mentre per un centro con feedback, un completamento potrebbe decrementare il numero di job nel centro di uno. Per l'inventory system con delivery lag esistono 3 tipi di eventi: un'istanza di domanda, una revisione dell'inventario e l'arrivo di un ordine. Anche in questo caso i 3 eventi sono tali perché potenzialmente possono cambiare lo stato del sistema. In particolare, una domanda diminuirà di uno il livello del bene nell'inventario, una revisione può aumentare la quantità del bene da ordinare e l'arrivo di un ordine aumenterà il livello dell'inventario del bene e diminuirà la quantità del bene da ordinare. Attenzione a non confondere la revisione con un evento che non fa cambiare lo stato. Faccio un esempio: nell'inventario vogliamo mantenere un numero di macchine tra le 10 e 30 unità. Supponiamo di avere come stato del sistema 20 macchine. Per arrivare a 30 mi basterebbe ordinare 10 unità. Ma se nella stessa settimana mi arriva un ordine di una macchina, il numero di macchine nell'inventario passerà a 19 ma l'ordine lo continuerò a fare di 10 macchine. Ecco che la revisione farà aumentare la quantità di unità da ordinare di uno passando quindi a 11 unità per l'ordine che avevo piazzato!

>Non è necessario che un evento causi a cambiamento nello stato del sistema. In un centro a servente singolo con feedback immediato, il completamento del servizio di un job modifica lo stato del sistema solo se il job non fa feedback. E per un inventory system con delivery lag, una revisione dell'inventario cambierà lo stato del sistema solo se viene effettuato un ordine!


> [!important] Definizione clock simulazione
> La variabile che rappresenta il valore corrente del tempo simulato in un modello di simulazione next-event è chiamato clock di simulazione.


> [!important] Definizione scheduling eventi
> Se lo scheduling degli eventi è utilizzato con un meccanismo di avanzamento del tempo next-event come base per sviluppare un modello di simulazione a eventi discreto, il risultato è chiamato modello di simulazione next-event.

Per costruire tale modello occorre:
- costruire un insieme di variabili di stato che insieme forniscono una descrizione completa del sistema.
- identificare i tipi di eventi nel sistema.
- costruire una collezione di algoritmi che definiscono il cambiamento di stato che avverrà quando ogni tipo di evento occorre.



> [!important] Definizione lista degli eventi
> La struttura dati che rappresenta lo scheduling temporale delle occorrenza per il prossimo evento possibile per ogni tipologia è chiamata lista degli eventi.

Spesso ma non necessariamente, la lista è rappresentata da una coda di priorità ordinata per il successivo tipo di evento che occorrerà.

## Algoritmo per next-event simulation
Un modello di simulazione next-event consiste nei seguenti quattro step:
1. Inizializzazione: il clock della simulazione viene inizializzato, solitamente a zero, e, guardando avanti, viene determinata e programmata la prima occorrenza in cui si verifica ogni possibile tipo di evento, inizializzando così la lista degli eventi
2. Processamento del evento corrente: la lista degli eventi viene scansionata per determinare quello più l'evento più imminente possibile, il clock della simulazione viene quindi avanzato fino all'istante programmato di questo evento e lo stato del sistema viene aggiornato per tenere conto del verificarsi di questo evento. Questo evento è noto come evento **corrente/attuale**.
3. Scheduling nuovi eventi: i nuovi eventi se esistono, che sono "spuntati" fuori dal evento corrente vengono piazzati nella lista degli eventi in ordine cronologico.
4. Conclusione: Il processo di avanzamento del clock della simulazione da un evento al successivo continua finché non viene soddisfatta una condizione terminale. Questa condizione terminale può essere specificata come uno pseudo-evento che si verifica una sola volta, al termine della simulazione, basata sull'elaborazione di un numero fisso di eventi, oppure sul superamento di un determinato istante del clock della simulazione oppure la stima di una misura di output con una precisione prescritta.

La simulazione next-event viene inizializzata una volta sola all'inizio di una replica della simulazione, per poi alternare i passi 2 e 3 fino al raggiungimento della condizione terminale.

Poiché gli istanti di tempo degli eventi sono in genere casuali, il clock della simulazione funziona in modo asincrono. Inoltre, dato che i cambiamenti di stato si verificano solo in occasione di eventi, periodi di inattività del sistema vengono ignorati facendo avanzare il clock in ogni istante in cui avviene un evento.

---
# Coda a servente singolo
La variabile di stato $l(t)$ fornisce una caratterizzazione completa dello stato di una coda a servente singolo in cui $l(t)=0 \impliedby \implies q(t)=0\ and\ x(t)=0$ 
$l(t)>0 \impliedby \implies q(t)=l(t)-1\ and\ x(t)=1$

dove $l(t), q(t),\text{e } x(t)$ rappresentano il numero di job del centro, nella coda, ed in servizio rispettivamente al tempo $t > 0$. A parole se il numero di job in servizio è conosciuto, allora il numero di job in coda e lo stato, idle o busy, del server è anche noto. Dato che lo stato del sistema è caratterizzato da $l(t)$, quali eventi causano il cambiamento di $l(t)$? Tali eventi sono due: 
- un arrivo in cui $l(t)$ viene incrementato di 1.  
- il completamento di un servizio fa decrementare $l(t)$ di 1.
  
In conclusione, il modello concettuale del sistema consiste nella variabile di stato $l(t)$ e due tipi di evento associati, arrivi e completamento del servizio. Lo stato iniziale può avere valori interi non negativi, ma solitamente si utilizza $l(t)=0$ per rappresentare il sistema come vuoto e come primo evento un arrivo. Anche lo stato terminale può avere valori interi non negativi, ma è comune utilizzare lo stato di vuoto e quiete come stato terminale. Invece di specificare il numero di job processati, andremo ad utilizzare come criterio di terminazione l'istante di tempo $\tau$ dopo cui nessun nuovo arrivo avverrà. Ciò permette di concludere i job ancora nel sistema ma anche di non avere più arrivi, cosi facendo l'ultimo evento sarà un completamento di servizio. Occorre infine un meccanismo per denotare un evento come impossibile. Un modo è strutturare la lista di eventi in modo che possa contenere solo eventi possibili, soprattutto se il numero di tipi di evento è grande. Come alternativa, se il numero di tipi è basso, la lista può contenere si eventi possibili che impossibili, che vengono denotati con un tempo pari a $\infty$. Di seguito lo pseudocodice di una coda FIFO con next event simulation: ![[pseudo_next_event.png]]. Il programma risultante `ssq3.c` in questo caso a differenza di `ssq2.c`, non produrrà più statistiche job-averaged, ma statistiche time-averaged. In particolare, le statistiche vengono calcolate accumulando gli integrali di $l(s),q(s)\ e\ x(s)$ nel tempo, operazione valida dato che sono tutte e tre funzioni piecewise che cambiano valore al occorrere di un evento. 

Dato che `ssq2` e `ssq3` simulano esattamente lo stesso sistema, i programmi dovrebbero produrre anche le stesse statistiche. Produrle però richiede che entrambi i programmi processino esattamente la stessa fonte stocastica di job in arrivo e richieste di servizio. In particolare in `ssq2`, le variate vengono generate in ordine alterno, mentre in `ssq3` l'ordine non è noto a priori. Per produrre le stesse statistiche andiamo ad utilizzare in `ssq3`, la libreria `rngs`.

---