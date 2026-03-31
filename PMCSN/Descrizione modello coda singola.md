#pmcsn #magistrale 
[[Teoria delle code]]
Come detto il modello di riferimento è quello dei sistemi di code. Consideriamo la coda singola con una capacità finita e dunque i job che non vengono accettati saranno persi. Attenzione la probabilità di loss è indice di connettività nei sistemi di comunicazione, dunque può tornare utile.

Ci sono diversi parametri associati ad una coda quali:
- la politica dell'ordine di servizio: FIFO, LIFO, random o con priorità. Ricordando che la priorità ha senso se si conosce la dimensione dei job in modo da dare priorità ai job di dimensione più piccola, per aumentare il throughput e diminuire la congestione della coda.
- se i job sono preemptible o meno.
- se il server è work conservative, ovvero il server appena termina un job ne va a gestire un altro.
- $\mu$: il tasso di servizio delle richieste.
- $\lambda$: il tasso d'arrivo delle richieste.
- $\displaystyle S={1\over\mu}$: il tempo di servizio (service time).
- $T_Q$: il tempo nella coda (waiting time).
- $T_S=T_Q+S$: il tempo di risposta (response time), logicamente è la differenza tra istante di tempo in cui la richiesta è stata soddisfatta e istante di tempo in cui il sistema entra nella coda.
- $\displaystyle\rho={\lambda\over\mu}$: indichiamo l'utilizzazione o busy time ovvero la popolazione media del servente. 
- $N_Q$: il numero di richieste nella coda.
- $N_S=N_Q+\rho$: il numero di richieste nel sistema.
- $E[n]_t$: numero di richieste completate nell'unità di tempo ovvero il throughput se si considera nel tempo, che indichiamo con $X$.

Di tutte queste metriche di performance ovviamente noi tratteremo i valori medi, indicando con $E[x]$ il valore medio della metrica $x$. Al crescere di $\lambda$ le metriche relative ai tempi e numero di richieste in coda e nel sistema aumentano, mentre al crescere di $\mu$ le metriche si abbassano, si ha appunto un miglioramento delle performance.  Attenzione però, migliorare il tempo di risposta medio non significa che il throughput migliori. Vediamo un esempio ![[thr_cmp.png|300]] nella figura le due code hanno si tasso di servizio diverso, ma il throughput è pari a $\lambda$ in entrambe le code. 
Una coda si trova in <font color=red>equilibrio stocastico</font>, ovvero in regime stazionario se $\lambda<\mu\implies\rho<1$. Siano gli le distribuzioni degli interarrivi e del tempo di servizio deterministiche, cioè costanti. Quanto vale $T_Q$ e $T_S$? $T_Q=0$ e $T_S=S$ dunque possiamo affermare intuitivamente che le metriche su i tempi di risposta sono influenzate dalla varianza delle distribuzioni di tempi di servizio e interarrivi. Presa una coda di cui non conosciamo la distribuzione ma sappia che è in equilibrio, il suo throughput sarà $X=\lambda$, indipendentemente dal valore di $\mu$. In una coda non in equilibrio $\lambda>\mu$ dunque la coda non può smaltire più richieste che $\mu$ e la coda crescerà all'infinito. $X=\mu$ è anche il lower bound della coda. Nel caso in cui $\lambda<\mu$ diremo anche che si ha l'equilibrio tra flussi:


> [!info] Definizione equilibrio flussi
> Un sistema ha equilibrio dei flussi quando ha tassi d'ingresso e d'uscita ovvero i flussi in entrata ed uscita sono uguali.

> [!attention] Flusso d'uscita
> Il flusso d'uscita è calcolato subito dopo che il job esce dal servente.

> [!attention] Equazioni dei flussi
> L'equazioni dei flussi vanno scritte come flusso d'uscita = flusso in ingresso! Se vale l'uguaglianza il sistema è in equilibrio.

Nella modellazione si verifica se il sistema è in equilibrio o meno, ovviamente. Attenzione, nel sistema in equilibrio si hanno prestazioni migliori nei tempi di risposta medi e nella popolazione media ma non nel throughput! Se passiamo al caso di coda con capacità finita, quando arriva una richiesta e la coda è piena, la richiesta verrà persa ed il throughput non sarà pari a $\lambda$! Il throughput per una coda con capacità finita nel caso di equilibrio dei flussi, è pari a $\lambda'=\lambda(1-P(coda\ piena))$, in particolare utilizzeremo la catena di Markov per stimare il tempo in cui la coda è piena e spiega l'eguaglianza $\lambda'=\lambda(1-P(coda\ piena))$. ![[loss_thr.png]]

Supponiamo adesso che i job debbano accedere al servente per richiedere un numero di servizi $\ge1$. Indichiamo con $P_B$ la probabilità che un job chieda un altro servizio e quindi torni all'interno della coda. Assumiamo che la coda abbia capacità infinita, l'equilibrio dei flussi e l'omogeneità tra i job, ovvero tutti i job abbiamo lo stesso comportamento descritto, e non solo una partizione di essi. Assumere che $\rho={\lambda\over\mu}$ è errato. In questo caso il servente ha tasso di servizio pari a $\phi$, il flusso in entrata è dato da $\displaystyle\lambda'=\lambda+P_B\lambda'\implies\lambda'-P_B\lambda'=\lambda\implies\lambda'={\lambda\over(1-P_B)}$.  ![[thr_pb.png]] L'utilizzazione sarà dunque $\displaystyle\rho={\lambda'\over(1-P_B)\phi}$, $\rho=1$ se $\displaystyle\lambda'=(1-P_B)\phi\implies P_B=1-{\lambda'\over\phi}$ Esempio numerico: sia $\phi=0.6\ job/s$ e $\lambda'=0.5\ job/s$ allora $P_B=0.25$, dunque per evitare di avere $\rho=1$ bisogna avere una probabilità sotto al 25% che i job ritornino nella coda. Il tempo di servizio l'abbiamo definito come $E[S]={1\over\mu}$, dunque in questo caso vale $\displaystyle E[S]={1\over\phi}$? No! Il numero medio di iterazioni per un job è $\displaystyle{1\over(1-P_B)}$, sia $\displaystyle E[S']={1\over\phi}$ il tempo medio di un'iterazione del job, $\displaystyle E[S]={E[S']\over(1-P_B)}$ è il tempo medio di completamente del job.

---


