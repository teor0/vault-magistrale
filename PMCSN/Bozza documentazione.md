# Obiettivi
Valutare R0 (tempo di risposta medio), la sua varianza, i percentili e le utilizzazioni al variare di **intensità/durata dei picchi di arrivo** e della **variabilità dei tempi di servizio**
Verificare se lo Spike Server migliora le prestazioni rispetto al caso senza di esso
Determinare il **valore minimo di SI_max** che garantisce R0 ≤ R_target senza congestionare lo Spike Server

**Indicatori di prestazione (output):**

- R0, R_web e R_spike (media, deviazione standard, p95/p99)
- frazione di richieste instradate allo Spike Server
- utilizzazione di Web Server e Spike Server
- distribuzione di SI (numero di richieste concorrenti nel Web Server)
- numero di commutazioni di routing (misura delle oscillazioni)

# Modello concettuale
i job hanno ciascuno una domanda di servizio S. 
**Componenti:**
1. **Sorgente di arrivi non stazionaria**, per modellare le fluttuazioni a breve termine.
2. **Dispatcher** con regola di routing basata su SI.
3. **Web Server**, con capacità unitaria.
4. **Spike Server**, con capacità c_s (frazione di CPU, vertical scaling).

Disciplina di servizio: **Processor Sharing (PS)** sia nel Web che nello Spike Server Il testo parla di “richieste concorrenti in esecuzione”, che è proprio il PS
Routing: Se n_web < SI_max → Web, altrimenti → Spike
Arrivi: iperesponenziale cattura così fluttazioni burst di breve durata e fluttuazioni sul lungo termine. CV=4 e media =0.15

**Variabili di stato:** stato MMPP (L/H); insieme dei job attivi in ciascun server, con lavoro residuo; modo di routing (Web/Spike); orologio di simulazione.


Eventi e avanzamento del tempo (next-event)

Arrivo: Genera S. Se n_web < SI_max, il job va al Web, altrimenti allo Spike. Pianifica il prossimo arrivo.
Completamento Web/spike: Rimuove il job, registra il tempo di risposta, aggiorna le statistiche.
