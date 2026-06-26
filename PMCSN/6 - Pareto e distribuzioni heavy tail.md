#pmcsn #magistrale 
[[5 - Ripasso distribuzioni]]
# Affidabilità come tempo residuo di vita
Le distribuzioni in cui $P(X>s+t|X>s)$ decresce al crescere di $s$ sono dette increasing failure rate. In questo caso, il dispositivo ha sempre più probabilità di guastarsi col passare del tempo. Le distribuzioni in cui $P(X>s+t|X>s)$ cresce al crescere di $s$ sono dette <font color=red>decreasing failure rate</font>. In questo caso, il dispositivo ha sempre meno probabilità di guastarsi col passare del tempo. Un esempio di decreasing failure rate è quello delle CPU, che hanno una probabilità di guastarsi più alta all'inizio del loro ciclo di vita mentre con il passare del tempo la probabilità scenderà. 
Sia $X$ una variabile aleatoria continua con funzione di densità di probabilità $f(t)$ e distribuzione $F(t)$, allora $r(t)$ definita come $\displaystyle r(t)={f(t)\over F(t)}$ dove $F(t)=1-F(t)=P(X>t)$. Consideriamo la probabilità che una lampadina dopo $t$ anni si guasti nei prossimi $dt$ secondi, allora: $P(X\in(t,t+dt)|X>t)=\displaystyle{f(t)dt\over F(t)}=r(t)dt$ è la probabilità di guasto istantanea. In particolare se $r(t)$ è costante, cioè $r(t)=\lambda$ allora $f(t)$ è un'esponenziale, questo perché vale la proprietà d'assenza di memoria!

# Distribuzioni heavy tail
Perché il tempo residuo di vita è così importante? Perché nei sistemi distribuiti la migrazione di un job, è un'operazione cruciale e il tempo restante per finire il processamento del job, influenza altamente le prestazioni del sistema. Introduciamo quindi il concetto di dimensione dei job e tutto ciò che è connesso ad esso.
- Per size del job, intendiamo la domanda totale di CPU.
- Per age del job, intendiamo l'uso totale della CPU fino all'istante corrente.
- Per lifetime del job, intendiamo la quantità totale richiesta di CPU dal job. Da non confondere con la size.
- Per remaining lifetime del job, intendiamo la quantità rimanente richiesta di CPU da parte del job.
  

> [!important] Nota bene
> Ad ogni istante $t$ non conosciamo la remaining lifetime del job, ma conosciamo l'age del job!

Il numero di operazioni di una classe di job $Z$ è una variabile aleatoria mentre la capacità di operazioni nell'unità di tempo di una CPU, indicata con $C$ non è una variabile aleatoria. Dunque $E[S]={Z\over C}$. Supponendo di voler analizzare la job lifetime in Unix per 3 mesi, solamente per job che hanno lifetime maggiore di 1 secondo si nota come $F(X)=P(X>x)$ dove $x$ sono i secondi di lifetime, ad ogni incremento unitario del tempo, il failure rate non è costante ma decrescente. In particolare il decreasing failure rate è circa $\displaystyle {1\over x}$ si ha dunque una "coda pesante" non più trascurabile come nell'esponenziale. ![[heavy_tail.png]] Questo tipo di distribuzioni dette appunto heavy tail sono modellate tramite la distribuzione di <font color=red>Pareto</font>: $\displaystyle {f(x)=\alpha k^\alpha x^{-\alpha-1}}$ con $k\le x<\infty$, $0<\alpha<2$. Se $\alpha>2$ la coda è più piccola dell'esponenziale. Con $\alpha\rightarrow 0$ si ha più variabilità e code più pesanti mentre con $\alpha\rightarrow 2$ si ha meno variabilità e code meno pesanti. L'i-esimo momento è finito per $\alpha>1$ dunque $E[x]=\displaystyle{\alpha k\over \alpha-1}$ per $\alpha>1$ e $Var[x]=\displaystyle{\alpha k^2\over (\alpha-1)^2(\alpha-2)}$ per $\alpha>2$ ma dato il nostro intervallo d'interesse $0<\alpha<2$ abbiamo un problema di varianza infinita!
Come soluzione utilizziamo la Pareto troncata/bounded in cui tutti i momenti sono finiti. La distribuzione in questo caso è data da: $\displaystyle f(x)=\alpha x^{-\alpha-1}{k^\alpha\over 1-({k\over p})^\alpha}$ con $k\le x\le p$, $0<\alpha<2$ e $C^2=\displaystyle{Var[x]\over E[x]^2}$ è compreso tra 25 e 49! Per calcolare $k$ possiamo partire dall'equazione $\displaystyle E[S]={\alpha k\over\alpha-1}\implies k=\displaystyle{\alpha-1\over\alpha}E[S]$. In conclusione possiamo elencare le proprietà della Pareto:
1. è decreasing failure rate
2. ha varianza infinita per $\alpha>2$ nella versione non limitata.
3. Una minuscola frazione di job più grandi comprende metà del carico del sistema. Infatti la Pareto nasce in ambito sociologo, dove l'1% della popolazione ha metà della ricchezza globale :) Mentre nell'informatica per $\alpha=1.1$ l'1% dei job occupa metà del carico del sistema.
   
Tornando alla migrazione dei job nei sistemi distribuiti, adesso capiamo che anche se un job "vecchio" possa richiedere un alto costo di migrazione, in quanto ha accumulato un sacco di memoria, se è davvero "vecchio" allora data la proprietà delle decreasing failure rate, ha un'altra probabilità di dover utilizzare molta CPU nel futuro, il che significa che il costo di migrazione può essere ammortizzato su un lifetime molto lungo.

## Esercizio confronto tempo di risposta

Sia $E[S]=0,5s$ e $\rho=0,5$ confrontiamo il tempo d'attesa $E[T_S]$ tra le distribuzioni ![[confronto_tempi_attesa.png]] ![[grafici_confronto_tq.png]]

---