[[3 - Descrizione modello coda singola]]
#pmcsn 
# Legge di Little
Assunto che la coda segua la disciplina FIFO, la capacità del nodo servente sia infinita e <font color=red>la coda sia in equilibrio</font> andiamo ad enunciare la legge di Little, molto importante per la sua elevata applicabilità. Ricordiamo che in equilibrio $X=\lambda$. Da notare che anche se decadono le assunzioni fatte, la legge resta molto applicabile. Prendiamo il sistema come fosse una scatola nera, in cui con $\lambda$ indichiamo il tasso d'arrivo, con $T$ indichiamo il tempo medio di soggiorno nella scatola nera e con $N$, indichiamo la popolazione media nella scatola nera. La legge afferma che se il sistema è **stabile**, la popolazione media è data dal flusso d'arrivo medio, moltiplicato per il tempo medio di soggiorno che i job passano nella scatola nera: $N=\lambda T$. Tramite la legge possiamo definire diversi risultati:
- se nella scatola nera c'è un intero sistema a coda, la legge è applicata alla popolazione media del centro: $E[N_S]=\lambda E[T_S]$
- se nella scatola nera c'è solo la coda, la legge è applicata alla popolazione media della coda: $E[N_Q]=\lambda E[T_Q]$
- se nella scatola nera c'è il servente, la legge è applicata alla popolazione media del servente, ovvero l'utilizzazione: $\rho=\lambda E[S]=\displaystyle{\lambda\over\mu}$
- se nella scatola nera c'è una rete di code, comunque siano interconnesse la legge si applica all'intera rete: $N=\lambda T$

Da Little quindi possiamo ricavare due risultati molto utili su i tempi di risposta, ovvero $\displaystyle=E[T_S]={E[N_S]\over\lambda}$ e $\displaystyle=E[T_Q]={E[N_Q]\over\lambda}$ ^4dbc71

---
# Notazione di Kendall ed equazione di Khinchin Pollaczek
La notazione che utilizzeremo per i nostri modelli è la notazione di Kendall: $A/S/m/B/N/D$ in cui:
- A: è la distribuzione degli interarrivi.
- S: è la distribuzione di servizio.
- m: è il numero di serventi.
- B: è la capacità della coda.
- N: è la dimensione della popolazione.
- D: è la disciplina della coda.

Per le distribuzioni indichiamo con $D$, la distribuzione deterministica, $M$, la distribuzione esponenziale, $E_k$, la distribuzione k-Erlang, $H_2$, la distribuzione iperesponenziale e con $G$ una distribuzione generale o meglio utilizzeremo la distribuzione di Cox. In particolare ci fermiamo solo su i primi 3 membri della notazione. Ad esempio il modello $M/G/1$ ha distribuzione esponenziale per gli interarrivi, distribuzione generale per il servizio e servente singolo.
Lo scheduling che utilizzeremo sarà non-preemptible ed astratto, quindi FIFO, random e LIFO non-preemptible, dove per astratto intendiamo che non si basa sulla dimensione dei job per definire la politica di scheduling. Ricordiamo che tutte queste politiche hanno tempo di risposta medio esattamente uguale. Preso un sistema $M/G/1$, data una qualunque distribuzione del tempo di servizio, <font color=orange>arrivi Poissoniani</font> e una qualsiasi politica di scheduling astratto la popolazione media nella coda è data da: $\displaystyle E[N_Q]={\rho^2\over 2(1-\rho)}[1+{\sigma^2[S]\over E[S]^2}]$ dove $\displaystyle{\sigma^2[S]\over E[S]^2}=C^2$ ovvero il coefficiente di variazione al quadrato, che è la dispersione del tempo di servizio. Questa è l'equazione di Khinchin Pollaczek, che chiameremo KP. Applicando la legge di Little con la KP abbiamo che: ![[kp.png]] a seconda della distribuzione che studiamo il coefficiente di variazione al quadrato assume i seguenti valori:
- $C^2=0$ per la deterministica.
- $\displaystyle C^2={1\over k},\ k\ge1$ per la k-Erlang. Quindi k volte più piccola dell'esponenziale.
- $C^2=1$ per l'esponenziale.
- $\displaystyle C^2=g(p)={1\over 2p(1-p)}-1$ per l'iperesponenziale dove se $p=0.5$ ricadiamo nell'esponenziale mentre al tendere di $p$ a 1, $C^2$ aumenta. 

Sostituendo tali valori, ricaviamo la seguente tabella: ![[tab_kp.png]] Se consideriamo la popolazione media nella coda abbiamo i seguenti risultati: ![[kp_time_sensivity.png]] le stesse considerazioni possiamo farle per la popolazione del sistema. Per i tempi di risposta, $T_S$ e $T_Q$, lo stesso ordine si mantiene per i valori media ma non per la varianza. Per definizione la KP ha valenza per ogni disciplina di scheduling astratto, ovvero ![[kp_dis_sens.png]] Questa uguaglianza si ha anche per $N_S$ e per i valori medi di $T_S$ e $T_Q$. Per la varianza del tempo di attesa nella coda l'eguaglianza non vale! L'ordine dei valori è il seguente ![[kp_var_dis.png]]

Riprendendo [[2 - Teoria delle code#^05f9ec|l'esempio del tasso d'arrivo raddoppiato]], mostriamo tramite KP come raddoppiare il tasso di servizio porta a tempo di risposta dimezzato. Assumiamo che la distribuzione degli arrivi sia esponenziale, abbiamo che $\displaystyle E[T_S]=E[T_Q]+E[S]={\rho E[S]\over 1-\rho}+E[S]={E[S]\over 1-\rho}$, se $\lambda'=2\lambda$ e $\mu'=2\mu$ $\implies\rho'={\rho}$ dunque $\displaystyle E[T'_S]={E[S']\over 1-\rho'}\implies E[T'_S]={E[S]\over 2(1-\rho)}={E[T_S]\over2}$ come volevasi dimostrare.

---
