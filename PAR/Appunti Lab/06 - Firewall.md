Quando parliamo di sicurezza di reti parliamo di segmentare e segregare.
- Segmentare: vuol dire partizionare le risorse
- Segregare: andare a regolare cosa consentiamo all'interno della politica di segmentazione. Sapendo che ci sono delle regole impone i limiti su cosa passa

Le vlan sono una tecnica di segmentazione che funzionano a livello 2, servono a separare i domini di broadcast della rete ethernet, quando usiamo tanti ip in uno switch è utile.

Parliamo di firewall quando parliamo da livelli 3 in su. Il firewall viene usato per individuare dei dispositivi che implementano delle regole di segregazione per regolamentare in modo più preciso possibile il traffico che deve passare. Il firewall lavora a livell 3 e 4 solitamente.

Possono esistere dei firewall che lavorano anche a livelli applicativi. Il termine DPI per esempio sta per deep packet inspection e indica un firewall che legge informazioni anche del livello 7 (http, dns...) Deep packet perchè andiamo a vedere informazioni che sono più in profondo nel pacchetto.

Possiamo usare un altro termine "application layer firewall" che è sempre un firewall anche di livello 7 però diversamente dal DPI è un firewall che emula completamente l'applicazione, rieseguono l'intera logica applicativa, fanno un analisi molto più evoluta rispetto al DPI.

Anche i protocolli NAT possono essere usati nella sicurezza della rete. Può essere usato per segmentare reti IP.

In pratica, in una rete decidiamo cosa far passare e cosa no grazie al firewall.

**Firewall**
Un firewall è un software che dev'essere configurato a seconda di certe policy. Dobbiamo capire cosa consentire o cosa non consentire. Essendo software possono essere insicuri, i firewall devono essere sviluppati con politiche che renda il firewall sicuro. Devono essere anche piazzati correttamente, possono anche esserci più firewall.

**Security policies**
Abbiamo sempre due paradigmi:
- Negazione implicita: deny all, allow some. Fai passare solo tramite conferma. È l'approccio più usato. È più sicuro.
- Accettazione implicita: allow all, deny some. Il contraio.

**Firewall access control policies**
Esempio l'implicit negation dice annulla tutto il traffico in entrambe le direzioni però per esmepio puoi dire consenti il traffico nelle porte 80 e 443 TCP. Consenti il traffico che va nella porta 53 UDP. Consenti il traffico in ingresso delle porte FTP.

**Tipologie di analisi**
Abbiamo almeno due categorie interessanti:
- Stateless/static: Quando abbiamo delle policy di accettazione/negazione che possono essere applicate analizzando il singolo pacchetto ignorando gli altri pacchetti. Un esempio è accettare/rifiutare dei pacchetti provenienti da un indirizzo IP.
- Stateful: Non posso capire se accettare/rifiutare un pacchetto senza considerare i pacchetti ricevuti in precedenza. Un esempio si trova nel protocollo TCP dove possiamo accettare dei pacchetti (esempio SYN/ACK oppure dati) solo se prima ho ricevuto il pacchetto SYN oppure ACK.

Il firewall cerca di capire i pacchetti che appartengono allo stesso flusso di pacchetti.
A livello di memoria/calcolo costano molto di più rispetto all'approccio stateless.

Il firewall può essere distribuito in due modi: preinstallato in un hardware su un pc o etc che hanno architetture dedicate non general purpouse oppure software.
- I firewall si concepiscono come software/hardware che mettiamo in mezzo alla rete. Al giorno d'oggi è popolare usare un firewall a livello di host. 

**Proxy firewall**
Un proxy firewall esegue l'applicazione in modo completo e se abbiamo un applicazione che esegue TCP instaura due connessioni in modo da fare proxy firewall -> client e proxy firewall -> server per riuscire ad analizzare tutto ciò che succede. Sono molto costosi.

-------

Screening router: router che effettua screening, vlaida ciò che passa o meno.

Quando abbiamo un proxy firewall per fare un analisi approfondita di certe applicazione abbiamo un bastion host. Screening router e bastion host possono collaborare perchè il lo screening router esegue un analisi dei pacchetti più veloce ma meno approfondita. Prima il pacchetto passa dallo screening router che è meno approfondito e una volta dato l'ok anche il bastion host lo analizza e se tutto va bene il pacchetto viene accettato.

**DMZ**
Possiamo creare delle suddivisioni all'interno della nostra rete. Le DMZ sono reti soggette a basso controllo, reti untrusted.
Quando separiamo le reti in diverse zone effettivamente si svolge una segmentazione. Posso inserire uno screening router anche per separare le reti interne oltre che per dire che pacchetti passano.

La tabella di input ha tre hooks, tre catene:
- Input: Analizziamo i pacchetti in ingresso. Arriva del traffico e decido se farlo passare agli host o meno.
- Forward: Analizziamo i pacchetti inviati da router. Controlliamo i pacchetti inviati da un interfaccia all'altra.
- Output: Analizziamo i pacchetti in uscita. Anche questo hook serve per gli host

Iptables -t (filter) -v (verbose) -n (listing) --line-numbers (numero della regola)
Cancellare -d

Linux usa policy di allow implicito.

iptables -P INPUT DROP // serve per cambiare policy da allow a deny
anche OUTPUT/FORWARD DROP

Quando droppiamo se proviamo a pingare ovviamente è come se l'altro host non esistesse. Il traffico comunque arriva. Tcpdump agisce a bassissimo livello; noi non ci aspettiamo che arrivi traffico però possiamo rilevarlo e cosa il firewall scarta.

Nemmeno il loopback funziona.
iptables -A INPUT -i (INPUT) / -o (OUTPUT) lo -j ACCEPT abilita il loopback almeno possiamo pingare noi stessi.

iptables -A OUTPUT -o eth0 -s 192.168.0.1 -d 192.168.0.2 -j ACCEPT
Questo comando abilita la rete tra 192.168.0.1 e 192.168.0.2. Se vogliamo che il nostro host possa comunicare con tutta la rete allora esempio non mettiamo -d. Se l'indirizzo è dinamico allora non mettiamo -s. Possiamo anche usare l'opzione tcp --dport 80 in modo da accettare tutti i pacchetti che arrivano dalla porta 80 di tcp.

Col filtering dobbiamo gestire entrambe le direzioni sia input che output.
iptables -A INPUT -i eth0 tcp --sport 80 -j ACCEPT 

Così funziona ma potremmo consentire troppo, chiunque con la porta 80 può comunicare con il mio host. Dobbiamo quindi fare in modo di accettare pacchetti SOLO se provengono da una connessione che ho aperto in precedenza.

iptables -R INPUT 2 -i eth0 -p tcp --sport 80 -m state --state ESTABLISHED -j accept
L'opzione -m = module, va a dire ad IPtables per fare analisi più approfondite. Il modulo usato è quello state, esegue analisi stateful. -m ci fa usare --state. La keyword established fa accettare il syn/ack solo se abbiamo già visto il syn, accetta pacchetti solo se hai giò visto questa rete.