[[Pipeline]]
# Pipeline superscalari
Con l'introduzione del processore i486 viene introdotta la pipeline a 5 stadi da Intel, in cui si aveva un componente singolo per ogni fase ad esempio un componente dedicato a EX, con un organizzazione classica che comprendeva però due fasi di decode, in quanto Intel traduceva e traduce tutt'oggi operazioni RISC in CISC dato che i suoi processori sono CISC. Il microcodice gestisce il comportamento di un processore pipeline, tuttavia il modo in cui un programmatore scrive il software può avere effetti diversi sull'efficienza della pipeline. Ad esempio lo swap di due valori attraverso le operazioni 
```
XOR ax,bx
XOR bx,ax
XOR ax,bx
```
Questa fase è meramente CPU bound, quindi prima dell'introduzione della pipeline era molto utilizzato dato che permetteva di effettuare lo swap senza utilizzare un terzo registro. Indurre questi conflitti su i registri rallentano tutti quanti dato che c'è un'unica corsia. 
Ad esempio in C all'interno di un ciclo queste due istruzioni 
```
a=*++p
a=*p++
```
hanno un effetto non trascurabile in quanto nel primo caso si attende l'update del puntatore per l'assegnamento mentre nel secondo caso. 

Esistono istruzioni macchina che permettono di effettuare il flush della pipeline, cioè di inserire una sorta di "tappo". Ad esempio per la sequenza ABDC, se l'istruzione D è un flush, si attenderà finché tutte le istruzioni della sequenza sia finalizzate prima di iniziare il processamento di una successiva istruzione E. Nei processori x86, una di queste operazioni è `CPUID` che permette di recuperare l'id numerico del processore su cui si sta lavorando. D'altra parte, usando questa istruzione si è sicuri che nessuna istruzione precedente nel flusso del programma vero e proprio è ancora in esecuzione lungo la pipeline prima che le istruzioni successive al CPUID vengono recuperate. CPUID è anche riferita come istruzione serializzante; la serializzazione dell'esecuzione delle istruzioni garantisce che eventuali modifiche a flag, registri e memoria per i precedenti le istruzioni vengono completate prima dell'istruzione successiva viene recuperata ed eseguita. Intel introdusse le pipeline super scalari con il Pentium Pro nel 1995. Qui le fasi EX potevano usufruire del reale parallelismo grazie alla ridondanza e la differenziazione del hardware (multiple ALU, hardware differenziato per operazioni intere e floating point, ecc.) tuttavia visto che certe istruzioni richiedevano più cicli macchina andavano a rallentare tutta la pipeline, si adottò anche il modello OOO (out of order) teorizzato da Tomasulo per l'IBM 360. ![[time_span.png]]
Alla base c'erano due idee, il commit/finalizzazione delle istruzioni deve avvenire nel ordine specificato dal programma e le istruzioni indipendenti devono essere processate il prima possibile. In una OOO pipeline vi sono due fasi principali: l'emissione, ovvero l'iniezione di istruzioni nella pipeline e il "ritiro", ovvero il commit delle istruzioni che le rende visibile a livello ISA. Tra queste due fasi vi è una fase di esecuzione di mezzo, in cui differenti istruzioni possono sorpassarsi a vicenda. Prese ad esempio le istruzioni A e B, gli effetti sullo stato del hardware quando B sorpassa A, ma non effettua il commit perché A non ha ancora committato, ci sono eccome. Quindi, processori OOO possono generare eccezioni imprecise tali per cui lo stato del hardware si differente da quello che dovrebbe essere osservabile durante l'esecuzione delle istruzioni nel loro ordine originale. Possiamo dire che se x viene superato o meno da y, lo stato del processore non è osservabile in modo preciso. Vi sono diversi casi:
- La pipeline potrebbe aver già eseguito un'istruzione A che, lungo il flusso del programma, si trova dopo un'istruzione B che causa un'eccezione. 
- L'istruzione A potrebbe aver cambiato lo stato della micro-architettura, sebbene infine non committa le proprie azioni sulle risorse esposte dall'ISA. 
- La pipeline potrebbe non aver ancora completato l'esecuzione delle istruzioni precedente a quelle di B, quindi gli effetti collaterali esposti all'ISA non sono ancora stati rilevati visibile in caso di eccezione.

Su questi dettagli problematici è nato l'attacco Meltdown come vedremo.

---

