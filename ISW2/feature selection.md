#is2 #magistrale 
la feature selection nasce per:
- ridurre il numero di attributi per ridurre il costo di learning o meglio il costo d'addestramento del modello. nell'ambito dell'ingegneria del software, abbassare i costi dell'addestramento pragmaticamente è poco importante. se l'addestramento dura 5 ore, e viene effettuato di notte, comunque il mio business lavora durante la giornata e il modello avrà tempi di inferenza dell'ordine dei decimi di secondo.
- migliorare le performance del modello, il vero valore sta nel diminuire il numero di feature mantenendo un accuratezza equivalente, piuttosto che aumentare l'accuratezza scegliendo feature diverse.
- riduce il costo di misurazione delle feature (!)

in sostanza a noi interessa di ridurre i costi nella misurazione piuttosto che migliorare performance o abbassare i cost computazionali.
ricordiamo che desideriamo colonne indipendenti fra loro ma correlate fortemente con le variabili di interesse. 1° bookmark.

rimuovere le feature risulta conveniente se la feature è irrilevante oppure è correlata ad altre.

ci sono due metodologie per effettuare feature selection: filter e wrapper. filter si basa su approcci statistici per valutare una feature indipendentemente dal modello in uso. 
3° bookmark per infogain
wrapper è molto pratico dato che si basa su un classificatore e la valutazione delle sue performance in base a diverse feature. ovviamente un approccio esaustivo non è praticabile per la feature selection. dunque anche se non si valuta tutto il dataset e perciò si ottiene un valore peggiore di accuracy, utilizziamo un approccio greedy per poter cercare il set di feature migliore.
2° bookmark approcci greedy. 
nell approccio greedy ovviamente mi fermo ad un set di feature con la cardinalità minore. i valori dell'approccio greedy, sono ovviamente frutto del validation set.
backward search è ovviamente più costoso del forward, ed il forward porta a meno feature rispetto al backward search.
più sono confidente nelle feature più tendo ad utilizzare backward search meno sono confidente più tendo ad utilizzare forward search.

nel progetto scegliamo 20 feature e dopo eseguiamo feature selection se si pensa che sia ragionevole.

per filter manca l'interpretabilità dei risultati.

tutto il processo di feature selection va fatto per ogni classificatore, per ogni metrica d'interesse.

---
a Weka importa per il dataset solo la prima riga `@relation <nome dataset>`. tramite `@attribute` si specifica il tipo di un attributo ed i possibili valori se definiti. I dati sono poi elencati come per il formato csv sotto la label `@data`
in weka purtroppo non si può lavorare con scale ordinali, si consiglia quindi di modificare la scala in una binaria.
nom data permette di scegliere l'attributo 

zeroR è il classificatore dummy di weka se non vogliamo utilizzare kappa per capire la semplicità del dataset.

vedi 4° bookmark

i filtri per la feature selection stanno dentro meta. altrimenti si farebbe feature selection sul test set.

feature selection va fatto prima del balancing dato che il balancing cambia il dataset.