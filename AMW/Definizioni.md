#amw #magistrale 
# Cybersecurity e Malware
La Cybersecurity si definisce come

> [!info] Definizione Cybersecurity
> Difesa di infrastrutture critiche, calcolatori, dispositivi mobili, sistemi elettronici, reti e dati informatici da attacchi dannosi.

Tra i vari aspetti che interessano questo settore ci sono: la sicurezza delle applicazioni, sicurezza di rete, sicurezza delle informazioni, la sicurezza operativa, l'analisi e il recupero degli incidenti ed infine la formazione degli utenti finali. Il malware al giorno d'oggi resta il metodo di attacco con maggior incidenza in attacchi cyber. Ma cos'è un malware? La parola deriva dalla contrazione di <font color=red>malicious software</font>. Formalmente possiamo definirlo come:

> [!info] Definizione Malware
> Un programma (o parte di un programma) che interferisce con il normale funzionamento di un calcolatore e/o permette l'esfiltrazione di dati dal calcolatore stesso

Un altra possibile definizione è:


> [!info] Definizione alternativa di malware
> Ogni programma che viene eseguito all'insaputa, contro l'esplicità volontà o contro gli interessi dell'utente del calcolatore può essere definito "malware"

Ma questa definizione risulta un po' lasca, e soprattutto così facendo si considerano parti integrando di Windows come malware, cosa che ovviamente non è giusto? Per dover di cronaca il primo virus fu sviluppato in ambito sperimentale nel 1971 e si chiamava **Creeper**, mentre il primo virus che ha portato il suo creatore in tribunale nel 1988 è il **Morris Worm**. 

# Tipi di malware
Elenchiamo le principali tipologie di malware:
- **Adware e Malvertising**: Visualizzano pubblicità, corrompono le domande o le risposte sui motori di ricerca, intercettano i click sui banner pubblicitari
- **Spyware e Stealer**: Monitorano il traffico Web per catturare le preferenze dell’utente, esfiltrano informazioni critiche quali i dati degli account bancari, delle carte di credito e delle criptovalute
- **Loader**: Programmi semplici e di piccole dimensioni che consentono di scaricare dalla rete ed installare ogni altro tipo di malware
- **Backdoor e Remote Access Trojan (RAT)**: Consentono l’accesso remoto non autorizzato al calcolatore
- **Bot**: Inseriscono il calcolatore in una grande rete (botnet) controllabile da un unico server centrale. Utilizzata per lanciare attacchi di tipo Distributed Denial of Service (DDoS) e per cryptomining
- **Ransomware**: Cifrano alcuni file critici sul calcolatore per poi richiedere alla vittima il pagamento di un riscatto per decifrarli. Per il pagamento si utilizzano cryptovalute, minimizzando così le possibilità di rintracciare i responsabili
- **Clipper**: Attaccano le transizioni finanziarie basate su crittovalute: monitorano la clipboard di sistema e per qualunque pagamento sostituiscono l’indirizzo del ricevente il pagamento con un indirizzo appartenente all’autore del malware
- **Virus**: Il malware si aggancia a file del calcolatore e si auto-replica infettando altri file in locale. Si diffonde tramite la condivisione dei file
- **Worm**: Il malware è un programma a se stante che si auto-replica e si diffonde tramite rete (posta elettronica, WWW, condivisione P2P, social network, ecc.)
- **Trojan**: Il malware è un programma che si presenta come benigno e utile alla vittima
- **Phishing**: Il malware non ha capacità di auto-replicazione e non viene
trasmesso automaticamente tramite rete; al contrario, è la vittima stessa che è indotta a scaricare ed installare il malware per mezzo di tecniche di “ingegneria sociale”
- **Spearphishing**: Una particolare forma di phishing avente come obiettivo un particolare individuo o gruppo di individui
- **Rootkit**: un insieme di malware che hanno come scopo ottenere l'accesso ad un computer o parte di esso. Questi software, oltre a garantire tale accesso, si preoccupano di mascherare se stessi o altri programmi utili per raggiungere lo scopo.

È comunque un grave errore assumere che il malware sia diffuso soltanto nei sistemi desktop personali. Il rischio più grave in effetti è ignorare la minaccia che il malware può rappresentare in tanti diversi ambiti come i sistemi dedicati al controllo industriale o i sistemi embedded. Per il primo caso l'esempio regina è Stuxnet, malware realizzato dal governo americano per attaccare PLC (controller logici programmabili) utilizzati per l'arricchimento di uranio in Iran. Tale malware utilizzava 4 diverse vulnerabilità "zero-day" di Windows.

> [!info] Definizione zero-day
> Si chiama vulnerabilità zero-day, una vulnerabilità all'interno del OS che non è stata ancora scoperta e corretta.

Per quanto riguarda i sistemi embedded, i firmware al loro interno, possono presentare malware che sono difficili da individuare per l'utente finale, dato che vedono l'intero sistema come una black box. Un esempio è uno spyware contenuto in videocamere di sorveglianza che permetteva a utenti remoti di eseguire qualsiasi comando con privilegi di root e raccoglieva informazioni. Quindi **OCCHI APERTI**. Il malware si presenta sotto tante forme e tanti diversi modi di operare. Non esiste una soluzione definitiva!

---



