#is2 #magistrale 
[[Lezione 8 - Snoring]]
Dobbiamo introdurre il concetto di effort aware metrics.
Ogni volta che si valuta una metrica di accuratezza serve l'actual.
La prima effort aware metric che introduciamo è la PofBx: la percentuale di bug che uno sviluppatore può identificare ispezionando l'x% di linee di codice.
Date delle entità possiamo ordinare le entità in base alla percentuale di bugginess predetta dal classificatore. 1°bookmark

Probability of Bug PofB20 è la percentuale che il classificatore mi permette di identificare ispezionando il 20% del codice.
Più un classificatore trovava classi buggy nel 20%, più è accurato.

Due metodi di una stessa classe richiedono un'ispezione con stesso effort rispetto a due metodi di due classi diverse.

Normalized Probability of Bug, NPofBx tiene conto anche della dimensione della classe 
Data una linea random di un'entità, questa entità è la più probabile di essere buggy. 
Cambiare il ranking senza modificare il classificatore, porta a trovare il 66% di classi buggy, ovvero 2 su 3 come evidenziato nel paper.

2° bookmark

RQ2 mostra come innanzitutto tra PofB90 e NPofB90 il gain è basso dato che stiamo andando a considerare quasi tutto e come NPofBx è nettamente migliore. In RQ3 confrontiamo tutto il ranking dei classificatori attraverso il coefficiente di Spearman. 3° bookmark
Il ranking è fatto su dataset diversi. Il 12% è la percentuale di quanto il ranking dei classificatori con PofB è simile al ranking dei classificatori con NPofB, che ricordiamo essere migliore.
RQ4: ci chiediamo se il classificatore migliore con NPofB20 è anche il migliore utilizzando NPofB50, NPofB60, NPofB70 ecc.



