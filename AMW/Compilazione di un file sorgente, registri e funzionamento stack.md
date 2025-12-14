#amw  #magistrale 
[[Riepiloghi utili]]
# Cosa accade nella compilazione?
Il file sorgente quando viene compilata passa attraverso diverse fasi. Per prima cosa l'unità di compilazione si prende carico del file e se esplicitato fa una pre-compilazione. Per un sorgente C, si utilizza`gcc -E hello.c` che sostanzialmente espande il sorgente aggiungendo gli header file e le macro. Serve al compilatore per evitare errori quando va ad invocare funzioni che non sono definite da nessun parte, in nessun header file. Il precompilato sarà poi compilato dal compilatore che produrrà codice Assembly. Tale codice sarà passato al assemblatore che produrrà codice macchina e simboli definiti e non definiti dal programmatore. Il linker si occuperà di prendere tutti i file oggetti necessari per produrre l'eseguibile. Il codice macchina tra file eseguibile e file oggetto sono diversi data la mancanza degli indirizzi nel file oggetto. ![[compilazione.png]]
Il punto di ingresso dell'eseguibile non è il main, ma sono delle funzioni che servono per eseguire il [[Riepiloghi utili#^13e5e2 | processo]]. Le librerie <font color=orange>statiche</font> contengono file oggetto già pronti per il linker. Le librerie <font color=orange>dinamiche</font> o shared object (so) in Linux e DLL in Windows, sono procedure pronte per essere utilizzate. Per la `print()` si ha una call alla libreria dinamica *libc*. Le librerie dinamiche sono copiate quando si crea il processo all'interno dello spazio di indirizzi del processo. Ogni file oggetto è diviso in sezioni ad esempio: text, data, bss o rodata (read-only data), il linker in realtà mette insieme tanti file oggetto differenti per assemblare un file eseguibile accorpando tutte le sezioni dello stesso tipo in segmenti. 

L'architettura di riferimento del corso è Intel/AMD a 32 o 64 bit data la diffusione di malware sia a 32 che a 64 bit. Per quanto riguarda la sintassi di assembly useremo quella di Intel perché usata in Windows e Linux e non quella AT&T usata solo in Linux ricordando però le principali differenze tra le due sintassi:
- La posizione degli operandi. Esempio: MOV sorgente, destinazione per AT&T mentre MOV destinazione, sorgente per Intel
- Quantificazione degli operandi, ovvero la grandezza in byte dei dati. In AT&T ad esempio movb è usata per 1 byte, movh per 2, movl per 4 e movq per 8 byte. In Intel invece si usa MOV byte ptr A, B per memorizzare in un puntatore a 1 byte il dato B. Ci sono poi WORD, DWORD, QWORD per 2, 4 e 8 byte. Ovviamente in entrambi i casi se sposto da un registro ad un altro non c'è bisogno di specificare la quantità di byte.
- Nome dei registri. In AT&T per scrivere il nome dei registri debbo mettere % prima mentre in Intel devo solamente scrivere il registro.
- Costanti. Esempio se voglio usare il numero 5, debbo usare $5 in AT&T mentre basta solo 5 in Intel.
- Deferenziazione dei puntatori. Esempio: in AT&T si usano le partentesi (EAX). In Intel si usano le parentesi quadre \[EAX] ma dato che spesso viene abusato questo meccanismo anche senza parentesi si ha lo stesso significato. Quando voglio un indirizzo si usa $ per AT&T mentre per Intel si usa la keyword OFFSET.

## Registri a 32 e 64 bit.
A 32 bit abbiamo i registri: eax, ebx, ecx, edx, edi, e esi per uso generale. Poi ci sono i registri segmento che contengono i descrittori di segmento rispettivamente cs per code segment, ds per data, ss per stack mentre es, fs e gs sono registri segmento dedicati al programmatore. Poi c'è l'esp per lo stack pointer ovvero il puntatore dello stack, eflags che contiene i bit di stato:
- ZF: zero flag pari a 1 se il risultato di un operazione è pari a 0.
- CF: carry flag pari a 1 quando il risultato di un operazione è troppo grande o piccola per l'operando di destinazione.
- SF: sign flag pari a 1 quando il risultato di un operazione è negativa.
- TF: trap flag pari a 1 se si sta facendo debugging e quindi si vuole eseguire un'istruzione alla volta.

Infine EIP usato per l'istruction pointer, ovvero all'istruzione che segue. Per manipolare l'istruction pointer devo usare operazioni che fanno fare salti all'interno del codice (questo avviene in Intel/AMD in arm posso manipolare direttamente il flusso).
Il registro EAX è a 32 bit ma al suo interno posso scrivere i primi 16 bit riferendomi a AX, oppure 8 bit riferendomi al registro alto AH oppure al registro basso AL. Quando scrivo ad esempio 16 bit su AX i valori dei restanti 16 bit non viene sovrascritto. Per i registri ricordiamo il bit meno significativo è SEMPRE quello più a destra.
![[strutturaregistri.png|300x300]]


A 64 bit abbiamo i registri:  rax, rbx, rcx, rdx, rdi, rsi, rbp, da r8 fino a r15, rsp, rflags e rip. Vale lo stesso discorso di partizionamento per scrivere solo 32, 16, o 8 bit. Di seguito si propone una tabella riepilogativa dei registri:
![[tab_registri.png|400]]

Per accedere in memoria le istruzioni macchina in AT&T usano: c(b,i,s) dove c è l'offset, b la base, i l'indice e s size o scalare. Questo equivale a: $b+i*s+c$. In Intel si usano le parentesi quadre con al suo interno la formula perciò $[b+i*s+c]$. L'unica eccezione è l'istruzione <font color=red>LEA</font> (load effective address) utilizzata dai compilatori per calcolare l'indirizzo come dovesse andare in memoria, senza però andarci. Il compilatore usa molto questa istruzione perché per operazioni come EBX=4\*EAX+3 usa: LEA 3(%ECX,%EAX,4), %EBX singola istruzione macchina invece di più istruzioni. A 64 bit Linux e Windows usano i registri in modo differente per i parametri di una funzione, in particolare:
- Linux usa in ordine i registri: rdi, rsi, rdx, rcx r8 e r9 quindi fino a 6 parametri supportati dal settimo si usa lo stack.
- Windows usa in ordine i registri: rcx, rdx, r8 e r9 quindi fino a 4 parametri supportati dal quinto si usa lo stack.

## Funzionamento stack
PUSH è l'istruzione per mettere il contenuto di un registro nella cima dello stack, mentre POP è l'istruzione per togliere il contenuto dalla cima dello stack. Ricordando che lo stack è discendente, la PUSH sottrae qualcosa dal valore di ESP/RSP mentre la POP incrementa il valore di ESP/RSP. Lo stack è utilizzato in maniera implicita ogni volta che invoco una funzione, tramite l'istruzione CALL. CALL mette in cima dello stack l'indirizzo di ritorno. L'istruzione RET conclude una funzione, che in sostanza fa una POP e mette il risultato di EIP/RIP. Per l'architettura a 32 bit, lo stack viene usato per passare i parametri delle funzioni, ad esempio: int fun(int a, int b, int c). In particolare siccome lo stack cresce per indirizzi decrescenti le push dei parametri avverranno nell'ordine, push c, push b e push a. Lo stack serve anche per memorizzare le variabili locali della funzione. Di seguito uno schema riepilogativo della visibilità e persistenza delle variabili. ![[visibility.png]] quindi tutte le variabili automatiche stanno sullo stack. Occhio a non fare confusione con variabili static automatiche. Le costanti definite tramite const invece hanno solamente un vincolo di non sovrascrittura.
Finché le variabili definite in una funzione non sono inizializzate, non si effettua una push sullo stack ma si fa una SUB del valore di byte da allocare a ESP. Quando poi si assegna un valore ad una variabile si userà la MOV con il registro ESP, quindi ad esempio: MOVL $5, 4(%ESP). Per evitare di spostare ogni volta ESP per ottenere il valore delle variabili locali di una funzione il compilatore usa un registro apposito.
Il <font color=orange>frame pointer</font> è il registro EBP/RBP, viene usato appositamente dalle funzioni per facilitare la vita al programmatore e al compilatore. Oggigiorno il compilatore non è obbligato ad usare il frame pointer data l'enorme evoluzione nel corso degli anni. Quando si usa il frame pointer una funzione fa una sequenza di istruzioni fisse detto prologo:
```
pushl %ebp
movl %esp, %ebp
```
a cui seguono dopo solitamente le istruzioni di allocazione. In questo modo ottengo subito un riferimento fisso con EBP e posso utilizzarlo per recuperare i valori delle variabili locali senza dover muovere ESP. Grazie a EBP possiamo andare anche a recuperare i parametri della funzione. ![[ebpesp.png|200]] Si chiama frame pointer in quanto ogni frame (porzione dello stack) identifica una funzione ed ognuno ha un puntatore che punta a tale frame. Quando una funzione che usa il frame pointer termina vengono utilizzate le seguenti istruzioni specifiche detto epilogo:
```
movl %ebp, %esp
popl %ebp
ret
```
In questo modo si ribilancia lo stack. Essendo molto utilizzato prologo e epilogo si hanno delle istruzioni macchina "ssinonimi" ovvero enter e leave. Tuttavia se si compila un codice C, la enter non viene MAI utilizzata dato che enter riceve un parametro per funzioni ricorsivamente definite. In C questo non è possibile quindi si dovrebbe utilizzare `enter 0`, ma questa istruzione è più lenta di push e mov, quindi i compilatori non la usano. 
Quando una funzione restituisce un valore scalare, si usano i registri EAX o EDX che quindi non sono registri che saranno preservati. I parametri dallo stack li rimuove il chiamato o il chiamante nello specifico una funzione `stdcall` chi è invocato rimuove i parametri attraverso l'istruzione `ret numero` dove il numero della return serve ad aggiustare lo stack pointer, come ad esempio `ret 12` rimuove 12 byte dallo stack. Per funzioni `cdecl`, chi invoca la funzione rimuove i parametri. Esistono poi funzioni `fastcall` che permettono di definire quanti parametri e quali registri per essi utilizzare ma cambia da compilatore a compilatore, in particolare per *gcc* si può utilizzare `gcc -mregparam(3)` permette di usare i registri per i primi 3 parametri.

---