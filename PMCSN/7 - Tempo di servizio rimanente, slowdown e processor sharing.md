#pmcsn #magistrale 

[[6 - Pareto e distribuzioni heavy tail]]
# Tempo di servizio rimanente e slowdown
Tornando al modello a coda singola con singolo servente. Supponiamo che arrivi una richiesta e che nella coda trovi una popolazione $N_Q$ in coda ed una richiesta all'interno del servente. Per prima cosa vogliamo calcolare il tempo di servizio rimanente, affinché la prima richiesta nella coda venga presa dal servente. Questo tempo medio che indichiamo con $S_{rem}$ è pari a $E[S_{rem}]={\lambda\over 2}E[S^2]$. Per la distribuzione esponenziale $E[S^2]=2E[S]^2\implies E[S_{rem}]={\lambda\over 2}2E[S]^2=\lambda E[S]^2=\rho E[S]$ e ci torna dato che l'esponenziale è senza memoria, $\rho$ è la probabilità di trovare qualcuno all'interno del servente e $E[S]$ è appunto il tempo di servizio. Se volessimo calcolare adesso l'attesa della richiesta $E[T_Q]$, tornando all'ipotesi abbiamo una popolazione $N_Q$ che richiederà un tempo pari a $t_c$ quindi la richiesta che arriva dovrà attendere in coda pari a $t_c+S_{rem}$. Ma sappiamo tramite KP che per l'esponenziale $E[T_Q]=\displaystyle{\rho E[S]\over 1-\rho}={E[S_{rem}]\over 1-\rho}={1\over 1-\rho}E[S_{rem}]$ possiamo quindi pensare che $\displaystyle {1\over 1-\rho}$ abbia a che fare con $t_c$. Partendo dalla KP dimostriamo il risultato generale ![[dim_esrem.png| 300]]
In realtà $T_Q=t_c\bigoplus S_{rem}$ dunque se uno dei due termini è 0 il risultato è 0, mentre se è pari a 1 non da contributo. Se $E[S_{rem}]=0$ non c'è alcuna coda dunque il job che arriva verrà servito immediatamente. Se $\displaystyle {1\over 1-\rho}=1$ il job trova solamente il job che il servente sta servendo con un tempo di servizio rimanente, piuttosto basso da non dover aspettare. Dunque possiamo concludere che $E[T_Q]=\displaystyle {E[S_{rem}]\over 1-\rho}={{\lambda\over 2}E[S^2]\over 1-\rho}$ valga per sistemi $M/G/1$ quindi con una distribuzione generale. A questo punto possiamo affermare che $E[T_S]=\displaystyle E[T_Q]+E[S]={{\lambda\over 2}E[S^2]\over 1-\rho}+E[S]$ per $M/G/1$. 

Ricordando due definizioni per gli scheduling:
1. Una politica di scheduling è preemptive se un job può essere interrotto nel corso della sua esecuzione per poi essere ripresa in un secondo momento a partire dal stesso punto in cui è stato fermato. 
2. Una politica di scheduling è **work-conserving** se esegue sempre non rimane in attesa quando c'è un job nel sistema.

Il seguente teorema valida l'equazione $\displaystyle E[T_Q]={{\lambda\over 2}E[S^2]\over 1-\rho}$

> [!important] Teorema di Conway, Maxwell e Miller
> Tutte le politiche di scheduling non-preemptive e astratti, ovvero che non utilizzano le dimensioni del job per costruire lo scheduling, hanno la stessa distribuzione sul numero di job nel sistema.

Consideriamo adesso il tempo di risposta medio del sistema per job di dimensione $x$ che è pari a $E[T_S(x)]=\displaystyle E[x+T_Q(x)]=x+E[T_Q(x)]=x+{{\lambda\over 2}E[S^2]\over 1-\rho}$
per quanto riguarda i tempi di risposta, si desidera avere un attesa "equo" rispetto alla dimensione del job.

> [!important] Slowdown
> Definiamo slowdown $E[sd(x)]=\displaystyle{E[T_S(x)]\over x}=1+{{\lambda\over 2}E[S^2]\over x(1-\rho)}$ dove più grande è la dimensione meno i job grandi sono rallentati in proporzione, rispetto ai job piccoli.

dunque il tempo di risposta tende ad essere rappresentativo delle prestazioni di solo pochi job: quelli più grandi tendono a enfatizzare la performance dei job veramente grandi, poiché contano di più nella media, dato che il loro tempo di risposta tende essere il più grande. Lo slowdown invece, tende a rappresentare le performance della maggior parte dei job perché è dominato dalle performance del gran numero di job piccoli. 

# Processor sharing
Come detto desideriamo che $E[T_S(x)]$ sia piccolo per job di dimensione piccola. Ma per i job di cui non conosciamo la dimensione occorre utilizzare tecniche avanzate come la processor sharing. Lo scheduling della CPU è approssimativamente processor-sharing per due motivi: 
1. in un sistema multi-risorse è utile avere multipli job eseguiti simultaneamente dato che job diversi chiedono risorse differenti e tramite la parallelizzazione si aumenta il throughput.
2. il processor sharing è un buon modo per eseguire job piccoli velocemente, dato che non si conosce la size dei job.

La processor sharing dovrebbe esser meglio della FIFO per quanto riguarda $E(T_S)$, perché processor sharing si "libera" dei job più piccole più velocemente, e dovrebbe essere molto meglio della FIFO per quanto riguarda lo slowdown. In particolare:  ![[processor_sharing.png]] per $C^2>1$ PS è meglio di FIFO, ovvero la coda $M/G/1/PS$ è insensibile alla varianza della distribuzione del tempo di servizio $G$ in media. Per casi specifici però la PS può essere peggio della FIFO ad esempio per 2 arrivi simultanei che richiedono 1s di servizio per la FIFO $E[T_S]=1,5s$ mentre per la PS $E[T_S]=2s$!. Per la processor sharing si ha che $E[T_S(x)]=\displaystyle{x\over 1-\rho}$, $E[sd(x)]=\displaystyle{1\over 1-\rho}$ dunque tutti i job hanno lo stesso slowdown "equo". Tutti gli scheduling preemptive non-size-based producono lo stesso slowdown medio qualunque sia job size $E[sd(x)]=\displaystyle{1\over 1-\rho}$ che dipende solamente da $\rho$. Spesso si fa il confronto tra PS ed altre politiche di scheduling per valutare lo slowdown, considerando la PS sempre con la prelazione. Lo slowdown è meglio valutarlo quando c'è assunzione sulla job size e tale confronto non necessita altre assunzioni particolari. La LIFO-preemptive è meglio della PS, dato che fa solo 2 prelazioni mentre la PS ne fa di più!

## Esercizio slowdown
Data una distribuzione esponenziale sia $E[S]=0,5s$ e $\rho=0,5$ vediamo come lo slowdown decresce ma i tempi di risposta crescono. Date le size $x=\{0.05,0.1,0.15,\dots,0.5\}$, $E[T_S(0.05)]=0.55$, $E[T_S(0.1)]=0.6$ e $E[T_S(0.5)]=1$ mentre $E[sd(0.05)]=11$, $E[sd(0.1)]=6$, $E[sd(0.15)]=4.33$ e $E[sd(0.5)]=2$. Appunto come volevasi dimostrare  lo slowdown decresce ma i tempi di risposta.

---

