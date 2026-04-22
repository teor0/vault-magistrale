#magistrale #pmcsn 
[[9 - Code a servente singolo con classi di priorità]]
Vogliamo confrontare due configurazioni possibili per un sistema. Assumiamo un tasso d'arrivo medio $\lambda=1\ j/s$ ed una domanda media di $Z=4\times 10^5$ operazioni, in modo esponenziale e di cui non si conosce la dimensione. Le possibili configurazioni che confrontiamo sono:
1. Server con capacità $C=10^6\ j/s$
2. Dual-core con capacità di ogni core $C/2$

le richieste di qualità di servizio (QoS) sono $E[T_Q]<0.15\ s$ e per almeno il 35% degli arrivi $E[T_S]<0.5\ s$. Per quanto riguarda i QoS:
- per il server con capacità, $E[S]=Z/C=0.4s$, $\rho=\lambda E[S]=0.4$ $E[T_Q]={\rho E[S]\over (1-\rho)}=0.26667\ s$, siamo fuori. Se proviamo due classi con prelazione abbiamo: $\lambda_1=\lambda P_1=0.35$ $E[T_Q]^{abstract-P}=P_1\displaystyle{{\lambda_1\over 2}E[S^2]\over (1-\rho_1)}+P_2\displaystyle{{\lambda\over 2}E[S^2]\over (1-\rho_1)(1-\rho)}=0.22488\ s$ dunque $E[T_S]=E[T_Q]_1+E[S]=0.666>0.5$
- per il dual-core: $E[S_i]={Z\over C/2}=2E[S]=0.8$ $E[T_Q]_{Erlang}={P_Q E[S]\over 1-\rho}=0.15238\ s$. 

Se utilizziamo invece il multiserver con priorità ![[ex_mul_ser_prio.png|400]] e per il primo QoS ![[risultato_ex_mul_prio.png]]
Per $P_{Q1}$ abbiamo messo $\rho_1$! Dato che le altre code "non esistono per lei". Se ci fossero più di 3 classi la situazione si complicherebbe abbastanza e necessitiamo della convoluzione per calcolare le classi di mezzo.
>$P_Q1$ è quindi la probabilità che tutti i server siano occupati da job di classe 1! Che è minore di $P_Q$

Per quanto riguarda $E[T_S]$ non ha senso calcolare il valore, dato che già $E[S]=0.8$!