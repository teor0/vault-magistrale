#pmcsn #magistrale 
[[Code a servente singolo con classi di priorità]]
# Definizione processo di Markov
> Si dice processo stocastico una famiglia di variabili aleatorie $\{X_t:t\in T\}$ discrete o continue e definite sullo stesso
spazio di probabilità $(\omega,\mathit{F},P)$ dove $t$ rappresenta un indice e $T$ l’insieme dei suoi possibili valori.

Meno formalmente, un processo stocastico è una collezione di variabili aleatorie dipendenti dal tempo, ovvero una sequenza di variabili aleatorie. Sia $E=\{S_0,S_1,\dots,S_n\}$ lo spazio discreto degli stati, in cui ogni stato è frutto di un evento: servente vuoto, arrivo nella coda, ecc. 

> Una catena di Markov è un processo stocastico $\{X_n,n=0,1,2,\dots\}$ dove $X_n$ denota lo stato al istante di tempo discreto $n$ tale che $\forall n\ge0$, $\forall i,j$ e $\forall i_0,\dots,i_{n-1}$: $P\{X_{n+1}=j|X_n=i,X_{n-1}=i_{n-1},\dots,X_0=i_0\}=P\{X_{n+1}=j|X_n=i\}=P_{ij}$ dove $P_{ij}$ è indipendente dal istante di tempo e dalla storia passata.

Quindi la catena di Markov gode dalla proprietà di assenza di memoria, conta solo lo stato corrente. La probabilità degli stati è stazionaria $P\{X(t)=s_i\}=\pi(s_i,t)$ è la probabilità che all'istante $t$ mi trovo nello stato $s_i$ se lo spazio è finito ed il processo è irriducibile ed ergodico. Per ergodico si intende che ogni stato è ricorrente non nullo. Ricorrente perché posso uscire e ritornare nello stato in maniera aperiodica. Non nullo, ovvero il tempo per ritornare nello stato è positivo e finito. Irriducibile perché lo spazio non è divisibile. Ogni stato può essere raggiunto da qualsiasi altro.
Per stazionaria si intende che, dopo un intervallo di tempo che non si conosce, il sistema converge alle probabilità ovvero $\lim_{t\rightarrow\infty}\pi(s_i,t)=\pi(s_i)$. Dopo quale istante il sistema converge è l'argomento di interesse.

Un sistema a capacità finita è sempre stazionario.
![[mark_chain.png|300]]