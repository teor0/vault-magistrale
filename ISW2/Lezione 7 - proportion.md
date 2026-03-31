#is2 #magistrale 

vediamo come fare il labeling.
abbiamo due approcci: szz+blame oppure szz+la prima affected version/fix version in Jira

la opening version è la versione in cui il ticket è aperto
la fix version è la prima versione del codice senza il bug riferito nel ticket
l'affected version è la versione che comprende l'introduction version fino al commit del fix.
l'injected version è la prima versione che viene etichettata come buggy.
l'inject version può coincidere con l'opening version, se ciò avviene la fixed version è la versione dopo

i bug li consideriamo post release.

circa il 30% dei ticket non hanno una affected version associata.

consistenza tra affected version e opening version:
se l'av è dopo la creazione del ticket c'è un errore di consistenza.
l'opening version è sicuramente la prova più forte per la presenza della failure.

l'approccio con szz + affected version è la migliore combinazione.

l'approccio di proportion è piuttosto biased

tutte le classi nascono non difettose. per avere una classe buggy necessito di una prova, ovvero un id di un ticket per un commit sulla classe.
in szz se vado troppo indietro creo dei falsi positivi. un bug regressivo szz lo etichetta come falso positivo, ovvero una classe che viene toccata dal commit con id di un ticket che però non c'entra nulla, verrà etichettata come buggy.
ho falsi negativi se nel commit non ho l'id del ticket.

la proporzione tra introduction version e opening version è pari alla proporzione tra opening version alla fixed version.
opening e fixed version le conosco sempre. posso quindi stimare l'affected version.

prima nasce il ticket prima si effettua il fix.

approccio total: utilizzo tutti i difetti con affected version per calcolare P mentre per i difetti che non hanno affected version utilizziamo il P calcolato.
perché usare total: ho pochi ticket

cross-project estimation per il cold start.

perché non usare total: si rischia di etichettare in maniera irrealistica e si fa una sovrastima delle performance.

nel train set è bene essere realisti mentre nel test set è bene essere accurati.
dati troppo accurati fanno cadere in overfitting il classificatore. un altro problema è utilizzare dati troppo vecchi che potrebbero viziare il risultato. l'ideale sarebbe utilizzare dati recenti. moving window premi la qualità dei dati, ma non si sa la dimensione adeguata della window

SZZ_B va al primo blame SZZ_B+ va al primo blame oppure alla opening version.
per SZZ_RA il refactoring aware ignora i cambiamenti identificati come refactoring e va una commit più indietro fino a che non trova una che modifica le funzionalità.
RQ1 parla della presenza di AV e se è attendibile e valida proportion
RQ2 e RQ3 differenziano per l'unità di misura.

in generale proportion ha metriche di performance migliori di SZZ tranne che per il recall. perché recall è più alta in szz e non in proportion? perché szz tende a trovare più positivi di proportion dato che va più indietro nel tempo.

il testing set dovrebbe essere etichettato tramite total.

---
la tecnica di valutazione è una tecnica di stima. la misurazione dell'accuratezza di un modello ha senso per il futuro non per il passato.
la tecnica di valutazione migliore è quella che mi caratterizza dati passati e mi stima l'accuratezza su dati futuri.

ripasso training set e test set e tutti i problemi legati al campionamento dei dati.
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

training set realistico e test set accurato.

date 10 release farò 10 volte l'etichettature.