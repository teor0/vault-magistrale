#pmcsn #magistrale 
[[9 - Code a servente singolo con classi di priorità]]
# Definizione processo di Markov
> Si dice processo stocastico una famiglia di variabili aleatorie $\{X_t:t\in T\}$ discrete o continue e definite sullo stesso
spazio di probabilità $(\omega,\mathit{F},P)$ dove $t$ rappresenta un indice e $T$ l’insieme dei suoi possibili valori.

Meno formalmente, un processo stocastico è una collezione di variabili aleatorie dipendenti dal tempo, ovvero una sequenza di variabili aleatorie. Sia $E=\{S_0,S_1,\dots,S_n\}$ lo spazio discreto degli stati, in cui ogni stato è frutto di un evento: servente vuoto, arrivo nella coda, ecc. 

> Una catena di Markov è un processo stocastico $\{X_n,n=0,1,2,\dots\}$ dove $X_n$ denota lo stato al istante di tempo discreto $n$ tale che $\forall n\ge0$, $\forall i,j$ e $\forall i_0,\dots,i_{n-1}$: $P\{X_{n+1}=j|X_n=i,X_{n-1}=i_{n-1},\dots,X_0=i_0\}=P\{X_{n+1}=j|X_n=i\}=P_{ij}$ dove $P_{ij}$ è indipendente dal istante di tempo e dalla storia passata.

Quindi la catena di Markov gode dalla proprietà di assenza di memoria, conta solo lo stato corrente. La probabilità degli stati è stazionaria $P\{X(t)=s_i\}=\pi(s_i,t)$ è la probabilità che all'istante $t$ mi trovo nello stato $s_i$ se lo spazio è finito ed il processo è irriducibile ed ergodico. Per ergodico si intende che ogni stato è ricorrente non nullo. Ricorrente perché posso uscire e ritornare nello stato in maniera aperiodica. Non nullo, ovvero il tempo per ritornare nello stato è positivo e finito. Irriducibile perché lo spazio non è divisibile. Ogni stato può essere raggiunto da qualsiasi altro.
Per stazionaria si intende che, dopo un intervallo di tempo che non si conosce, il sistema converge alle probabilità ovvero $\lim_{t\rightarrow\infty}\pi(s_i,t)=\pi(s_i)$. Dopo quale istante il sistema converge è l'argomento di interesse.

Un sistema a capacità finita è sempre stazionario. Attenzione che per capacità del sistema intendiamo capacità del buffer/coda più servente o serventi. Dunque per coda singola con servente singolo è pari a C+1
![[mark_chain.png|300]]
Se volessimo calcolare $\lambda'$ con la catena di Markov, bisogna utilizzare le equazioni di bilanciamento: $\displaystyle \pi_0\lambda=\pi_1\mu\implies \pi_1={\lambda\over\mu}\pi_0$ $\pi_0$ si ottiene normalizzando $\displaystyle\sum^{C}_{i=0}\pi_i=1$ andando avanti si ha che $\pi_1(\lambda+\mu)=\pi_0\lambda+\pi_2\mu\implies \pi_2=({\lambda\over\mu})^2\pi_0$ dunque $\pi_C=({\lambda\over\mu})^C\pi_0$ e $\pi_i=({\lambda\over\mu})^i\pi_0$. Tornando a $p_0$, abbiamo che $\displaystyle\sum^{C}_{i=0}\pi_i=1\implies \sum^{C}_{i=0}({\lambda\over\mu})^i\pi_0=1\implies \pi_0={1\over \displaystyle\sum^{C}_{i=0}({\lambda\over\mu})^i}$
La probabilità di perdita è quella in cui il sistema si trova nello stato C, ovvero buffer pieno: $P_{loss}=\pi_C=\displaystyle{({\lambda\over\mu})^C\over \displaystyle\sum^{C}_{i=0}({\lambda\over\mu})^i}$ 
in definitiva $\lambda'=\lambda(1-P_{loss})$ e $\rho={\lambda'\over\mu}$ ![[ex_markov.png|300]]

# Sistema M/M/m/m
Per un sistema $M/M/m/m$ la situazione cambia: ![[mmmm_markov.png|300]] $\pi_0$ si ottiene sempre normalizzando e, in questo caso $\displaystyle{1\over i!}$ tiene conto delle combinazioni dei serventi oppure di quanti sono pieni. Nel caso generale andando anche a sostituire $\pi_0$ otteniamo quella che viene detta Erlang-B: ![[erlang_B.png|300]] che è l'analogo di $P_Q$ nella formula di Erlang. In particolare, confrontando la Erlang-C e la Erlang-B, ovvero il sistema con coda infinita e serventi multipli ed il sistema con perdita e serventi multipli ![[erlang_c_b_confronto.png|300]] abbiamo che:$\displaystyle P_Q={({\lambda\over \mu})^m \over m!(1-\rho)}p(0)$ e $\displaystyle\pi_m=({\lambda\over\mu}^m){1\over m!}\pi_0$
Isolando i termini $p(0)$ e $\pi_0$ tra i due termini, quello della Erlang-C è maggiore di quello della Erlang-B. Il sistema che ha maggior probabilità di diventare vuoto è ovviamente Erlang-B, perché non ha coda e quando un servente è libero la probabilità che arrivi un processo da servire è praticamente nulla, dato che è un evento simultaneo che dovrebbe accadere. Nei sistemi stazionari due eventi simultanei viene posta a zero, perché è molto rara. Accade in periodi limitati ma all'infinito è nulla.
D'altra parte però tra $p(0)$ e $\pi_0$, $\pi_0>p(0)$ e dunque questi esempi potrebbero bilanciarsi. 

---
