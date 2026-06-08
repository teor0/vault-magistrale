#pmcsn #magistrale 
Esercizio 1.1.2
La distinzione tra verifica del modello e validazione del modello non è sempre chiara nella pratica. Generalmente, per l'algoritmo 1.1.1, l'obiettivo finale è realizzare un modello di simulazione ad eventi discreti valido. Se ti dicessero che “questo modello di simulazione ad eventi discreti è stato verificato ma non è noto se il modello è valido” come interpreteresti tale affermazione?

Che è stato realizzato un buon modello consistente rispetto alle specifiche, ma che è assolutamente sconosciuta la sua validità rispetto al sistema reale. Insomma potrebbe essere un modello che non ha nulla a che fare con il sistema reale! D'altronde se un esperto stabilisse subito tramite l'osservazione del output della simulazione, che quei risultati sono errati, allora avrei un simulatore inutile! 

Esercizio 3.1.1 a)
I risultati passano da:
```
for 10000 jobs 
average interarrival time r= 2.0161 
average wait w............ = 3.8597 
average delay d........... = 2.3616 
average service time s.... = 1.4981 
average # in the node l... = 1.9143 
average # in the queue q.. = 1.1713 
utilization x............. = 0.7430
```
con Uniform(1,2)
a
```
for 10000 jobs 
average interarrival time r= 2.0161 
average wait w............ = 6.0337 
average delay d........... = 4.5407 
average service time s.... = 1.4929 
average # in the node l... = 2.9922 
average # in the queue q.. = 2.2518 
utilization x............. = 0.7404
```
con Exp(1.5) per i tempi di servizio. Le statistiche sono più alte perché la varianza passa da 1/12=0.083 a $({1\over 1.5})^2=0.44!$
punto b) tornando alla uniform(1.0,2.0) e portando i job a 100 mila otteniamo i seguenti valori:
```
for 100000 jobs 
average interarrival time r= 1.9990 
average wait w............ = 3.8473 
average delay d........... = 2.3471 
average service time s.... = 1.5002 
average # in the node l... = 1.9245 
average # in the queue q.. = 1.1741 
utilization x............. = 0.7505
```
insomma si ha una convergenza più "fine" ma i valori sono pressoché identici per il singolo punto di stima!
Esercizio 3.1.2 
I check applicabili sono $w=d+s$ e $l=q+x$ e sono verificati.
Esercizio 3.1.4
con uniform(1.3,2.3) si arriva a regime stazionario con circa 800 mila job. in particolare:
```
for 800000 jobs 
average interarrival time r= 2.0018 
average wait w............ = 9.9531 
average delay d........... = 8.1530 
average service time s.... = 1.8001 
average # in the node l... = 4.9720 
average # in the queue q.. = 4.0727 
utilization x............. = 0.8992
```
Esercizio 3.1.5
utilizzando la seguente GetService:
```
double GetService(void)
{
	long k;
	double sum=0.0;
	long tasks=1+Geometric(0.9);
	for(k=0;k<tasks;k++)
		sum+=Uniform(0.1,0.2);
	return (sum);
}
```
i risultati stazionari si ottengono dopo 50 mila job e sono verificati come voleva l'esercizio:
```
for 50000 jobs 
average interarrival time r= 2.0027 
average wait w............ = 5.7645 
average delay d........... = 4.2729 
average service time s.... = 1.4916 
average # in the node l... = 2.8777 
average # in the queue q.. = 2.1331 
utilization x............. = 0.7446
```
anche se tasso di arrivo, tasso di servizio ed utilizzazione sono gli stessi del caso in cui si usava l'Uniform(1,2) per il servizio qui si hanno statistiche diverse. Questo perché si ha una combinazione di due funzioni in questo caso la geometrica e la uniforme tra 0.1 e 0.2 che si portano ad avere la stessa media del caso Uniform(1,2), ma portano sicuramente ad una varianza più alta! D'altronde se usassi la KP dovrei utilizzare $E[S]^2$ o $C^2$, in cui la varianza gioca un ruolo, per calcolare il tempo d'attesa.