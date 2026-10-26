#soa #magistrale 
[[Attacco Spectre]]
# Loop unrolling
Questa è una tecnica software che consente di ridurre la frequenza dei branch durante l'esecuzione dei cicli e il relativo costo delle istruzioni di controllo del branch. Essenzialmente si basa sull'avere l'autore del codice o il compilatore ingrandire il corpo del ciclo inserendo più istruzioni che altrimenti sarebbero eseguite in diverse iterazioni del ciclo. Un esempio di loop unrolling inserito manualmente è:
```
#pragma GCC push_options 
#pragma GCC optimize ("unroll-loops") 
void _start(void){int i,j; for(j=0;j<(CYCLES>>16);j++){ 
	for (i=0; i<CYCLES; i++) { 
		a[i] = b[i] * c[i]; 
	} 
} 
exit(0); 
} #pragma GCC pop_options
```
Nelle versioni più recenti di `gcc`, si può utilizzare il flag `-O` per indicare il loop unrolling al compilatore. Attenzione però che l'unrolling potrebbe esercitare una maggiore pressione sull'utilizzo dei registri portando a interazioni di memoria più frequenti. Quando si fa affidamento su enormi valori di srotolamento, la dimensione del codice può crescere enormemente, di conseguenza la località e l'efficienza della cache potrebbero peggiorare in modo significativo. A seconda delle operazioni da svolgere, potrebbe essere meglio ridurre il numero di passaggi iterativi effettivi tramite la “vettorializzazione”, una tecnica che vedremo più avanti.

# Power wall e NUMA
Un processore potrebbe eseguire più velocemente attraverso un migliore sfruttamento dei componenti hardware e crescita dei transistor. aumentando così la frequenza del clock. Ma oggigiorno ci troviamo di fronte al power wall, che in realtà impedisce la costruzione di processori con frequenza più elevata. Infatti il consumo energetico cresce esponenzialmente con la tensione secondo la regola $V\times V\times F$ (e 130 W è considerato il limite superiore per dissipazione). Il modo che abbiamo per aumentare continuamente la potenza di calcolo di le singole macchine devono fare affidamento su unità di elaborazione parallele. Una prima opzione è utilizzare multiprocessori simmetrici: ![[multiproc_simmetrici.png]] dove le CPU singole hanno le cache dedicate e la memoria è condivisa. L'evoluzione è il Chip Multi Processor (CMP), dove all'interno di un chip si hanno due core che hanno la cache di primo livello dedicata ma quella di secondo livello condivisa quindi si hanno più CPU in un chip appunto ![[CMP.png]] ed infine l'ultima evoluzione, quella largamente presente oggi è il Symmetric Multi-threading (SMT) dove ogni CPU ha al suo interno più hyperthread ![[SMT.png]]. L'accesso in memoria su queste architetture risulta però il collo di bottiglia, inoltre se si garantiscono accessi scorrelati si ha lavoro indipendente. Nasce così il concetto di scalabilità della memoria, tramite <font color=orange>Non Uniform Memory Access</font> (NUMA) dove una CPU ha una zona di memoria dedicata "vicina" in cui le sue performance sono migliori, rispetto a zone "lontane" che richiedono più tempo per l'accesso ![[NUMA.png]] Infatti tra 50 e 200 cicli macchina sono impiegati in una zona NUMA vicina per leggere un valore, dove 50 cicli si impiegano per accedere alla cache e 200 alla RAM, mentre per una NUMA remota si hanno tra i 200 e 300 cicli, 200 per una cache condivisa e 300 per la RAM.  

---