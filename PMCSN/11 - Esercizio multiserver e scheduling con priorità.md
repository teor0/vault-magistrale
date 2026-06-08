#magistrale #pmcsn 
[[9 - Code a servente singolo con classi di priorità]]
# Esercizio multiserver
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

---
# Scheduling size based
Per quanto riguarda gli scheduling size based andiamo a considerare il caso in cui si voglia dare una priorità in base alla classe a cui il job appartiene, ovvero in base alla sua dimensione. Per il caso abstract abbiamo visto che Indipendentemente dalla classe $k$, il tempo di servizio medio per una classe è dato da $E[S]$. Purtroppo nel caso size based ciò non accade. Preso $S\in (x_{k-1},x_k]$, $\displaystyle E[S_k]=\int^{x_k}_{x_{k-1}} tf^n(t)dt$ dove $\displaystyle f^n(t)={f(t)\over F(x_k)-F(x_{k-1})}$, ovvero si normalizza la funzione di densità per renderla una media e informalmente possiamo dire che quanto più è grande l'area dell'integrale, quanti più elementi ci sono nella classe. Gli altri risultati sono che: $\lambda_k=\lambda(F(x_k)-F(x_{k-1}))$ $P_k=F(x_k)-F(x_{k-1})=\displaystyle{\lambda_k\over \lambda}$ e $\rho_k=\lambda_kE[S_k]=\lambda F(x_k)-F(x_{k-1})=\displaystyle\lambda\int^{x_k}_{x_{k-1}} tf(t)dt$. Per il criterio di priorità alle prime classi vale: $E[S_1]\le E[S_2]\le\dots \le E[S_r]$ però in generale l'ordinamento del $\lambda_r$ non lo conosco in quanto dipende dalle aree delle classi. Ovviamente se ho classi con intervalli equidistanti essi influenzano il valore. Ad esempio per distribuzioni monotone decrescenti, $\lambda$ ha un ordine che è monotono crescente, ovvero $\lambda_1\ge \lambda_2\ge\dots \ge\lambda_r$, infatti per l'esponenziale si hanno due serie opposte, con tempo di servizio decrescente e tassi d'arrivo crescenti: $E[S_1]\le E[S_2]\le\dots \le E[S_r]$ e $\lambda_1\ge \lambda_2\ge\dots \ge\lambda_r$. 
Un altro risultato interessante è che $\displaystyle\sum^{k}_{i=0}\rho_i=\lambda\int^{x_k}_{0} tf(t)dt$ dato che da 0 a $x_k$ ho valori continui e la somma di integrali continui è un singolo integrale tra i due estremi. Quindi con un'analogia al caso abstract, il tempo di attesa per la classe $k$ è dato da $E[T_{Q_k}]=\displaystyle{{\lambda\over 2}E[S^2]\over (1-\displaystyle\sum^{k}_{i=1}\rho_i)(1-\displaystyle\sum^{k-1}_{i=1}\rho_i)}={{\lambda\over 2}E[S^2]\over (1-\displaystyle\lambda\int^{x_k}_{0} tf(t)dt)(1-\displaystyle\lambda\int^{x_{k-1}}_{0} tf(t)dt)}$
Per quanto riguarda il tempo medio d'attesa globale possiamo sempre calcolarlo come $\displaystyle E[T_Q]=\sum^{r}_{k=1}P_kE[T_{Q_k}]$ ma essendo $P_k=F(x_k)-F(x_{k-1})$ possiamo calcolarlo direttamente come ![[tq_sb_prio.png|300]]
In conclusione possiamo dire che ![[sb_prio_vs_abs_prio.png|300]] dato che il denominatore nel caso size base è maggiore uguale a quello del caso abstract o meglio $\displaystyle\sum^{h}_{i=1}\rho_i^{SB}\le\sum^{h}_{i=1}\rho_i^{abs}, \forall h$

Questo perché in $\displaystyle\sum^{h}_{i=1}\rho_i^{abs}$ $E[S_i]$ sono tutti pari a $E[S]$, mentre per $\displaystyle\sum^{h}_{i=1}\rho_i^{SB}$ abbiamo una serie decrescente $E[S_1]\le E[S_2]\le\dots \le E[S_r]$

> [!important] Varianza ridotta
> I tempi d'attesa miglio sono frutto dello scheduling size based, che riduce la variabilità! 

Per quanto riguarda il tempo di servizio medio della classe $k$ non posso dire nulla su quale scheduling tra size based ed abstract sia il migliore. Per magari le prime classi la size based performa meglio ma mentre nelle ultime performa peggio dello scheduling abstract. 
Per quanto riguarda però il tempo di risposta medio globale, essendo $E[S]$ invariante dal caso size base al caso abstract posso affermare che ![[ts_sb_vs_abs.png|300]] essendo $E[T_S]=E[T_Q]+E[S]$, ma essendo ![[sb_prio_vs_abs_prio.png|300]] la disuguaglianza rimane valida.

---
# Scheduling shortest job first, size based con prelazione e shortest remaining
Se facciamo tendere il numero di classi $r$ all'infinito, otteniamo quello che viene detto scheduling size based, shortest job first ![[sjf.png]] dato che il numeratore per $r\rightarrow\infty=dF(x)$ ed una sommatoria nel continuo è proprio l'integrale. Tornando invece allo scheduling size based, la prelazione ha senso se presa una richiesta $h$ nel servente, $E[S_k]<E[S_{h}]$ mentre se la richiesta in corso è più veloce di quella in coda non ha senso. Per analogia con il caso abstract preemptible possiamo calcolare il tempo d'attesa della classe $k$ nella size base con prelazione. Ricordando che nel caso abstract al numeratore avevamo $\displaystyle{1\over 2}\sum^{k}_{i=1}\lambda_iE[S^2]$ ovvero il tempo d'attesa rispetto alle prime $k$ classi che precedono. Ma nel caso size based ciò è diverso. Il tempo di servizio rimanente per job non preemptible è pari a $\displaystyle{\lambda\over 2}\int^{x_k}_{0}t^2dF(t)$ ed il tempo di servizio rimanente di job preemptible è pari a $\displaystyle(1-F(x_k)){\lambda\over 2}x_k^2$ dove $(1-F(x_k))$ è la probabilità che il job sia di classe $z\ne k$ con $z=\{k+1,\dots,r\}$ si hanno i seguenti risultati ![[sb_preemptible.png|300]] Anche in questo caso il tempo d'attesa globale può essere calcolato come la somma pesata oppure direttamente come ![[sb_prelazione_tq.png|300]] Se facessimo un confronto tra size based con prelazione e shortest job first, non sapremmo dire quale risulterebbe migliore. Questo perché da un lato, l'impatto della variabilità su politiche FIFO nella size based con prelazione porta a prestazioni peggiori, e dal altro nella SJF un job molto grande in servizio, funge da "tappo" e senza prelazione l'attesa è "eterna". In conclusione:

> In presenza di varianza bassa la size based con prelazione ha prestazioni migliori.

Da qui l'intuizione, se aggiungessimo la prelazione alla SJF? Nasce così la <font color=orange>Shortest Remaining Job First</font>, utilizzata nei web server in presenza di sovraccarico, in cui in ogni momento il server lavora sul job con il tempo di processamento rimanente più breve. La politica SRPT è preventiva in modo al arrivo di un nuovo job l'esecuzione del job corrente viene interrotta se il nuovo job ha un tempo di esecuzione più basso, del tempo di esecuzione rimanente del job corrente. SRPT raggiunge il tempo medio risposta più basso possibile su ogni sequenza d'arrivo.
L'equazione si ottiene anche in questo caso portando all'infinito la size based con prelazione: ![[srjf.png|300]] esiste poi la variante condizionata dalla size in cui si ottengo le seguenti equazioni: ![[srjb_sb.png|300]] in particolare il tempo d'attesa è il tempo che un job di size $x$ attende, prima di ricevere servizio ed il secondo termine della somma è il tempo dal momento in cui il job riceve il servizio per la prima volta fino al suo completamento (tempo di permanenza). In particolare il rapporto $\displaystyle{dt\over 1-\rho_t}$ è la lunghezza del periodo di busy. Dunque il tempo di permanenza del job $x$ è proprio la somma di tutti questi periodi di busy.

## MJQM
MJQM è un modello proposto nei datacenter di Google in cui un job ha una domanda di servizio e numero di core in parallelo necessari per eseguire. Dunque il job di classe $i$: occupa $n_i$ server in parallelo per $S_i$ ore, dove $S_i$ è una variabile aleatoria. Lo stato del MJQM non è il solo numero di job, ma quanti sono in esecuzione. 
![[MJQM.png|300]] 

---