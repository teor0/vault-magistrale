#is2 #magistrale 
# Valutazione tramite machine learning
È importante capire quanto un modello è accurato, perché esistono centinaia di modelli ed io voglio usarne solo 1 e tramite la validazione se utilizzare il modello o meno. La tecnica di valutazione è una tecnica di stima. La stima ha come obiettivo stimare l'accuratezza del modello in futuro. 
La misurazione dell'accuratezza di un modello, ha senso per il futuro non per il passato. 

> [!info] Base
> Io devo fare il miglior uso di dati passati che ho, per capire la sua accuratezza su dati futuri che non ho.

La tecnica di valutazione migliore è quella che mi caratterizza un modello, utilizzando dati passati sulle performance che avrà  su dati futuri. 
la tecnica holdout è la prima vediamo. la stratificazione assicura che ogni classe è rappresentata proporzionalmente in entrambi i dataset.
il problema del holdout è che il campione del test set non si rappresentativo della difficoltà
nel k-folds si dimostra che il valore migliore di k è 10
per i dati che dipendono dal tempo esistono le tecniche di validazione time-series in cui i dati devono rispettare un ordine temporale.
la tecnica walk-forward si presta bene all'ingegneria del software. moving window è sempre preferibile a walk-forward però non conosco la dimensione della window. 
data drift: i dati hanno subito un cambiamento, uno slittamento nel peso. 
anche in questo caso non esistono soluzioni perfette, ma posso addestrare più modelli per stabilire qual performa meglio.

kappa misura quante volte il modello è meglio di un classificatore dummy
un classificatore dummy è un classificatore che predice sempre la classe maggioritaria. il classificatore dummy sarà la nostra baseline.
kappa va da -1 a 1 in cui lo 0 è dummy. se kappa tende a -1 è peggio del dummy, se kappa tende a 1 è meglio del dummy. in genere kappa arriva a 0.4. un kappa >0 valida anche la teoria che sta dietro.
la coppia di valori di precision e recall sono sensibili alla threshold dei valori considerati positivi.

risenti roc curve e threshold.

date 10 release farò 10 volte l'etichettature.
