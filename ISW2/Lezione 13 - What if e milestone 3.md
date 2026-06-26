abbiamo visto come i predittori aiutano a misurare qualcosa nel futuro, ma anche per avere stime di cosa sarebbe accaduto se le cose fossero andate diversamente.
vediamo come il classificatore frutto della milestone 2 "reagisce" su classi del progetto modificate. come visto in precedenza ![[studio_sc.png]] le violazioni impattano sulla quantità di difetti.
nella milestone 3 ci chiediamo quanto impatto hanno gli smell sulle classi buggy. what if il numero di smell sarebbe stato 0?
l'unico feature che variamo è il numero di smell, le altre restano invariate.
l'unica feature che poniamo pari a 0 è il numero di smell e tutte quelle feature correlate o frutto di ciò le altre rimangono invariate.
feature actionable: è una feature che può essere considerata in modo isolato e modifica (vedi definizione online)

feature non actionable: linee di codice

calcola la correlazione tra code smell e bugginess slide 10 what if

gli smell dovrebbero essere calcolati all'inizio della release. per la release 1 gli smell non impattano dato che non c'erano prima. (unico caso)

si consiglia di calcolare gli smell, al ultimo commit della precedente release.
metriche di complessità si dovrebbero calcolare all inizio mentre metriche di processo alla fine.

dato che il classificatore deve predirre qualcosa, occorre per prima cosa creare un dataset fittizio in cui il numero di smell è =0.
dato A il dataset originale, partizioniamo A in B e C. In B in realtà è frutto della manipolazione di B+ ovvero la porzione di A il cui numero di smell è >0. Mentre C è la porzione di A con numero di smell pari a 0.

nel dataset C la porzione di A con numero di smell pari a 0, le 69 classi non le avrei potute evitare ma per il dataset B a fronte di 66 classi buggy, il dataset sintetico predice 38 classi buggy se non avessi mai avuto smell. quindi il numero di classi buggy che avrei potuto evitare è 66-38=28 (bookmark 3 min 38)
quanto presente nel dataset C, indica già quanto gli smell non impattano dato che si hanno 69 classi buggy anche se ho 0 smell.

ragionevolmente ci si aspetta un 20% nel progetto più o meno.

quanto facciamo il labeling diciamo che una classe è buggy non quando inizia ad essere buggy. lo smell impatta l'inizio di essere buggy. bookmark 4

nella milestone 3 abbiamo solo A come training set, non training e test set.

nel report per la milestone 1 e 2 ci si aspetta una più ampia discussione sulla metodologia applicata mentre per le milestone 3 e 4 ci si aspetta una più ampia discussione su i risultati.

nei threads to validity ci finiscono le assunzioni ed i trade-off.

per non scrivere codice in più
in weka>explorer>classify è possibile fare supplied test set per calcolare la predizione su A, B e C. ad esempio per B+ il 38 esce dalla somma degli elementi della prima colonna della confusion matrix.
nel report va messo quanto fatto