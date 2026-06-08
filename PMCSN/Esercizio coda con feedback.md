# Contesto 
Si consideri un centro di servizio a servente singolo con meccanismo di feedback. ![[feedback.png]]
Si richiede di determinare una possibile soluzione analitica del sistema, sapendo che:
- il tempo di servizio singolo medio segue una distribuzione uniforme U(1.0,2.0); 
-  il tasso d'arrivo è $\lambda=0.5\ job/s$;
-  probabilità di feedback $\beta=0.20$;

# Soluzione analitica proposta
Il tempo medio di servizio del centro è pari alla media della distribuzione uniforme: 
$\displaystyle E[S]=\overline{x}={a+b\over 2}=1.5\ s$. 
Il tasso di servizio del centro risulta essere: 
$\displaystyle\nu={1\over E[S]}=0.\overline{6}\ job/s$.
Grazie all'equazione di traffico $\lambda'=\lambda+\lambda'\beta$, ricaviamo che il flusso in uscita è
$\displaystyle\lambda'={\lambda\over 1-\beta}=0.625$
mentre l'utilizzazione del centro è pari a 
$\displaystyle\rho={\lambda'\over\nu}=0.9375$
Il numero di cicli medi nel centro è pari a 
$\displaystyle{1\over 1-\beta}$ 
dunque possiamo calcolare il tempo di servizio medio per un job che effettua il feedback come: 
$\displaystyle E[S]_F={E[S]\over 1-\beta}=1.875\ s$. 
L'idea adesso è quella di approssimare il centro con feedback come un semplice centro a servente singolo dove però il tempo di servizio medio ed il tasso d'arrivo sono $E[S]=E[S]_F=1.875\ s$ e $\lambda=0.5$. 
A questo punto si può utilizzare come distribuzione del tempo di servizio un'uniforme $U_1(1.0,2.75)$ oppure $U_2(0,3.75)$ in quanto in entrambi i casi la media è pari proprio a $1.875$. Per "correttezza" si sceglie $U_1(1.0,2.75)$ poiché mantiene un limite inferiore pari a quello della distribuzione "originale", ma si svolgeranno i calcoli anche per l'altro caso. Passiamo quindi a calcolare la varianza per applicare successivamente la KP per ricavare il tempo d'attesa medio nel centro. In particolare per la prima distribuzione: 
$\displaystyle Var[S]_1={(b-a)^2\over 12}={(2.75-1.0)^2\over 12}=0.255208$ 
mentre per la seconda: 
$\displaystyle Var[S]_2=1.171875$ 
di conseguenza 
$E[S^2]_1=Var[S]_1+E[S]^2=0.255208+3.515625=3.770833$  
$E[S^2]_2=Var[S]_2+E[S]^2=4.6875$ 
Utilizzando la KP otteniamo i due tempi d'attesa:
1) $\displaystyle E[T_Q]_1={{\lambda\over 2}E[S^2]_1\over 1-\rho}={0.942708\over 0.0625}=15.083328\ s$
2) $\displaystyle E[T_Q]_2={{\lambda\over 2}E[S^2]_2\over 1-\rho}={1.171875\over 0.0625}=18.75\ s$

e rispettivi tempi di risposta $E[T_S]_1=E[T_Q]_1+E[S]=16.958328\ s$, $E[T_S]_2=E[T_Q]_2+E[S]=20.625\ s$

---