#is2 #pmcsn 
[[14 - Simulazione]]
# Numeri casuali e generatore di Lehmer
Tuttavia con l'uso delle tracce la simulazione "vale" fino ad un certo punto, soprattutto se non si ha certezza della bontà delle tracce. Per avere risultati più concreti ci avvaliamo delle distribuzioni probabilistiche. Per generare le variabili aleatorie utilizziamo trasformazioni matematiche per passare da numeri a variabili.
L'idea è quindi generare numeri casuali tra 0 e 1 per trasformarli tramite funzioni inverse. I moderni algoritmi generatori sono ampiamente accettati in quanto soddisfano i seguenti criteri:
• casualità - l'output passa tutti i test statistici sulla casualità
• controllabilità - ovvero la capacità di riprodurre l'output
• portabilità - capacità di produrre lo stesso output su una grande varietà di sistemi
• efficienza - veloce e poco oneroso per quanto riguarda le risorse necessarie
• documentazione - teoricamente analizzato e testato 

Un generatore di numeri casuali ideale, produce un output tale che: ogni valore nell'intervallo $0.0 < u < 1.0$ ha la stessa probabilità di verificarsi. Un buon generatore di numeri casuali produce un output che è (quasi) statisticamente indistinguibile da un generatore ideale. Presa un'urna in cui all'interno ci sono tutti i numeri interi da 1 a $m-1$: $X_m=\{1,2\dots,m-1\}$, quando estraggo un bussolotto con il numero $x$, calcolo la probabilità come $\displaystyle u={x\over m}$. I possibili valori che estraggo sono quindi $\displaystyle{1\over m},{2\over m},\dots,{m-1\over m}$ dove ad esempio se $m=7$, $X_7=\{1,2,3,4,5,6\}$.
È importante che $m$ sia abbastanza grande in modo da poter generare con alta densità, valori distribuiti tra 0.0 e 1.0. Da notare inoltre che 0.0 e 1.0 sono valori impossibili da generare e la stessa probabilità per ogni estrazione implica la sostituzione di l'elemento estratto. 
Per ragioni pratiche si effettuerà l'estrazione senza sostituzione. Se $m$ è grande e il numero di estrazioni è piccolo rispetto a $m$, la distinzione è in gran parte irrilevante.

Il generatore pseudo-casuale di Lehmer è definito da due parametri fissi $m$, il modulo che è un numero primo intero e $a$, moltiplicatore che è un intero. La sequenza di interi che si generano è definito dall'iterazione: $x_{i+1}=g(x_i)$ dove $g(x_i)=ax\ mod\ m,\ 0\le g(x)< m$ e $x_0\in X_m$ è definito come il <font color=green>seme</font> (seed) del generatore che non influisce sulla frequenza generata. $m$ è un numero primo per evitare che $g(x)\ne 0$ e che la sequenza si "rompa". 

> [!info] Irrilevanza del seed
> Presi due generatori di Lehmer in cui $m=7$ $a=3$, dati due seed diversi la sequenza generata non cambia, ma il punto d'inizio si.

Se il moltiplicatore e il modulo sono scelti correttamente, il generatore di Lehmer è statisticamente indistinguibile dal estrarre con sostituzione da $X_m$. La scelta di $m$ è dettata, in parte, da considerazioni di sistema. In generale, vogliamo scegliere $m$ come il più grande intero primo rappresentabile. 


> [!important] Teorema
> Se la sequenza $x_0, x_1, x_2,\dots$, è prodotta da un generatore Lehmer con moltiplicatore $a$ e modulo $m$, allora $x_i=a^ix_0\ mod\ m$ con $i=0,1,2,\dots$

NOTA: è assolutamente una pessima idea calcolare $x_i$ calcolando prima $a_i$.

> [!important] Teorema
> Se $x_0\in X_m$  e la sequenza $x_0, x_1, x_2,\dots$ è prodotta da un Lehmer generatore con moltiplicatore $a$ e modulo $m$, allora c'è un intero positivo $p$, $0<p\le m-1$, tale che $x_0, x_1, x_2,\dots, x_{p-1}$ sono tutti diversi e $x_{i+p}=x_i$ con $i=1,2,\dots$ la sequenza è periodica con periodo $p$. In aggiunta $(m-1)\ mod\ p=0$.

La scelta di $a$ è importante per generare tutti i numeri della sequenza. Se si è in grado di generare tutti i $m-1$ numeri $a$ è detto <font color=purple>full period</font>. Se scegliamo un seed iniziale $x_0\in X_m$ e generiamo la sequenza $x_0, x_1, x_2,\dots$ allora $x_0$ occorrerà di nuovo. $x_0$ rioccorrà al indice $p$ pari a $m−1$ o un divisore di $m−1$. Siamo interessati a moltiplicatori full-period in cui  $p=m-1$. Moltiplicatori full-period generano una lista circolare con $m-1$ elementi distinti. ![[molt_periodici.png|300]]
Attenzione che per il generatore $a,m=(7,13)$ è peggiore dato che la sequenza generata presenta coppie dove $i$ è il doppio di $i+1$. In particolare se ordiniamo i numeri della sequenza, otteniamo un grafico detto _lattice_.
![[lattice_graf.png|300]]


> [!important] Overflow
> Attenzione che il prodotto $ax$ potrebbe essere pari a $a(m-1)$ e se è impossibile rappresentare interi maggiori di $m$, avviene overflow.

Dunque per evitare problemi si sceglie $m$ numero primo, tale che $m=aq+r$, dove $q=\lfloor m/a\rfloor$ $r=m\ mod\ a$. In particolare valori di $r<q$ permettano implementazioni senza overflow, dette <font color=orange>modulo-compatibile</font>.

Riscriviamo quindi $g(x)$ come $g(x)=ax\ mod\ m=\gamma(x)+m\delta(x)$ dove $\gamma(x)=a(x\ mod\ q)- r\lfloor x/q\rfloor$ e $\delta(x)=\lfloor x/q\rfloor-\lfloor ax/m\rfloor$. Da notare come l'operazione modulo viene effettuata prima dei prodotti.


> [!important] Teorema
> Se $m=aq+r$ è primo e $r<q$, per $x\in X_m$ e $\delta(x)=0$ oppure $\delta(x)=1$ dove $\delta(x)=\lfloor x/q\rfloor-\lfloor ax/m\rfloor$. Si ha che $\delta(x)=0$ se e solo se $\gamma(x)\in X_m$ $\delta(x)=1$ se e solo se $-\gamma(x)\in X_m$ dove $\gamma(x)=a(x\ mod\ q)- r\lfloor x/q\rfloor$

L'algoritmo è dunque:
```
t=a*(x%q)-r*(x/q) //t=gamma(x)
if(t>0)
	return r;    //delta(x)=0
else
	return (t+m); //delta(x)=1
```

Il moltiplicatore $a$ è "piccolo" se $a^2<m$ e se è piccolo $a$ è modulo compatibile, in particolare tutti i moltiplicatori da 1 a $\lfloor\sqrt{m}\rfloor=46340$. Se $a$ è modulo compatibile, $a$ non è necessariamente piccolo, ad esempio $a=48271$ è modulo compatibile ma non è piccolo.
Ovviamente cerchiamo moltiplicatori full-period e modulo compatibili e grazie all'algoritmo in `rng.c` abbiamo un generatore pseudo-causale robusto che soddisfa tutte le caratteristiche che abbiamo discusso.

---


 