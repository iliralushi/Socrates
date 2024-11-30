Il livello trasporto introduce il concetto di comunicazione fra le applicazioni eseguite all'interno dell'host. 

TCP è solo uno dei protocolli utilizzati nello stack, ed è il più importante. L'altro protocollo di trasporto è UDP. È un protocollo molto semplice, aggiunge relativamente poco ad IP, giusto il necessario per far si che la comunicazione non avvenga tra host ma tra applicazioni.

Per comunicare fra applicazioni su host è necessario serve una multiplazione e demultiplazione tra processi quindi capire dove inviare il pacchetto.
Inoltre aggiungiamo il rilevamento dell'errore attraverso l'uso di checksum. Nell'IP il checksum proteggeva solo l'header, nel protocollo UDP lo abbiamo per tutto il pacchetto. Questo check può essere disattivato.

Alcuni servizi aggiuntivi di TCP sono
- Il trasferimento affidabile dei dati: (anche solo il fatto che ci sia un controllo dell'intero pacchetto). TCP cerca di adattare la velocità in cui invia i pacchetti, quindi aumenta/rallenta qunado è possibile.
- Controllo di flusso e congestione: funzionalità che cercano di regolare il tasso di invio dei segmenti da parte del mittente.

ESEMPIO PAGINA 8
Gestire la multiplazione dei processi vuol dire esaudire questo schema. Ci dev'essere un meccanismo che ridirige i processi nel modo giusto. L'SO deve capire quale pacchetto IP deve andare da una parte e quale dall'altro.

**Porte**
Una porta è un informazione per gestire la multiplazione a livello applicato implementato dal livello trasporto TCP/IP. L'header ha al suo interno una porta sorgente (porta dell'applicazione che ha inviato il pacchetto) ed una porta destinazione (porta associata a chi riceverà il pacchetto). La coppia ip/porta identifica l'applicazione in esecuzione su un certo host in tutto Internet

**Paradigma client/server**
Parliamo di client/server solo da questo livello. È la forma con cui un utente ed un destinatario possono comunicare tra di loro. A livello trasporto dobbiamo prevedere l'esistenza di quello che chiamiamo server e quello client.

A livello IP il pacchetto veniva sempre ricevuto, bastava una connessione via cavo e gli host comunicavano direttamente, qui non è così.

- Processo server: Un processo server deve richiedere l'apertura di una porta per dire che vuole mettersi in ascolto (il server vuole aprire una listening port/connessione passiva). Vuole dire al sistema operativo "io mi metto a disposizione per gestire i dati che riceverai". Quando il pacchetto arriva su una stessa porta il processo server prende i dati. Se il processo non viene registrato allora scarterà il pacchetto.
- Processo client: Un processo cliente deve richiedere l'apertura di una connessione attiva. Indica la porta di destinazione in cui è in ascolto il processo server. Viene associato ad una porta per gestire le risposte correttamente da parte del processo server.

**ESEMPIO PAGINA 16**
Abbiamo un client ed un server.
Il server si mette in ascolto, il client quando vuole comunicare si mette in ascolto nella porta (client port, server port). I messaggi server-client hanno come porta sorgente server porta destinazione client, i messaggi client-server il contrario.

**Categorie di numeri di porta**
Non tutte le porte sono uguali. Le porte sono grandi massimo 2 byte, quindi i valori vanno da 0 a 65535 (2^16-1)... Ci sono 3 categorie di porte.
- 0-1023: **Well known ports; queste porte sono associate a processi server che si mettono in ascolto per ricevere processi standard, servono per attivare servizi.** Possono essere utilizzati solo da processi con privilegi di root o simili.
- 1024-49151: Registered ports; L'uso di queste porte è principalmente per gli utenti della rete, non esistono vincoli restrittivi. Devono essere approvate da IANA e possono essere usate da qualsiasi processo.
- 49152-65535: Dynamic ports; porte non utilizzate da processi server, sono porte completamente libere. Usate spesso per il client.

Dalla 1023 alla 1024 cambia molto, il resto non cambia tantissimo.

**Porte note**
nano /etc/services

Se una porta è in ascolta nello stack TCP (es 50000) allora non ci saranno conflitti se anche una porta UDP 50000 è in ascolto. Per identificare la comunicazione a livello 4 mi servono 5 elementi;
- ip mittente 
- ip destinazione 
- porta mittente 
- porta destinazione 
- protocollo trasporto (che deve essere uguale)

Quando un processo server si mette in ascolto ed usa un protocollo ben noto utilizza la porta well known. Quando un processo client vuole comunicare il numero di porta viene scelta casualmente. La divisione tra porte alte e porte basse viene utilizzata per evitare conflitti tra la comunicazione tra client e server.

**NOTA 1**
Il packet flow identifica tutti i pacchetti che appartengono ad una stessa comunicazione. Ogni volta che vogliamo identificare il fatto che tutti i pacchetti appartengono allo stesso flusso a livello 4 avranno la stessa quintupla per identificarli.

Per applicazioni semplici bastano la quintupla, a volte questo concetto non è sufficiente, ad esempio nel peer2peer vengono aperte più porte.

**Protocollo UDP**
Cosa non ha: 
- Servizio di consegna non garantito, qundi non ha affidabilità. Ha un servizio di tipo best effort; i pacchetti possono essere persi duplicati consegnati senza un ordine preciso.
- Servizio senza connessione. Non abbiamo un legame tra mittente e destinatario nel pacchetto UDP. Quando invio un pacchetto udp non controllo nemmeno se il server esiste.

Con UDP finchè non invio dei dati non succede nulla, quindi esattamente come se non si sa del legame tra client e server. In TCP è l'opposto, si ha un tentativo di creare una connessione tra i 2.

**Pacchetto UDP**
L'header è composto dalle porte (src/dest) - c'è il campo lunghezza quindi quanto è grande il pacchetto e poi c'è il campo checksum infine ci sono i dati.

Il checksum a livello trasporto viene calcolato non solo sull'header e sul payload ma anche sullo pseudo-header di UDP. Lo pseudo-header indica il fatto che usiamo alcune informazioni del livello 3; indirizzo ip mittente destinatario, protocollo (il protocollo utilizzato nello stack sopra il trasporto) la lunghezza udp etc. Il checksum viene fatto su tutto.

In IPV6 non è presente un checksum a livello 3 dato che il livello 4 rileva anche gli errori presenti nel livello 3. Viene considerato ridondante.

Un router che fa nat utilizza in maniera intensiva le porte per gestire indirizzi pubblici/privati. Usiamo le porte per effettuare multiplexing tra indirizzi privati.

Il calcolo del checksum è una somma binaria a 16bit. Se i bit sono 11111111111 allora no errore se diverso c'è errore.

In UDP/TCP gli algoritmi di checksum sono scelti per essere eseguiti su hardware general-purpouse. È il primo checksum nello stack tcp/ip a garantire integrità end-to-end. Gli algoritmi CRC in H2N per esempio non lo faceva.

**Protocollo TCP**
Come per udp dato che è un protocollo nel livello trasporto estende le funzionalità del protocollo IP per permettere la comunicazione tra due proecssi applicativi.

Riseptto ad udp il protocollo tcp aggiunge:
- La garnazia di inviare pacchetti.
- Un paradigam di comunicazione orientato alla connessione e orientato allo scambio di flusso di dati. Non ha un interfaccia che gestisce i pacchetti, oscuriamo il fatto che i dati vengono inviati sottoforma di pacchetti, astraiamo.

**Trasferimento affidabile TCP**
Non vogliamo rendere la rete affidabile. Se la rete viene compromessa siamo in grado di rilevare il problema ed agire ciò vuol dire affidabile in TCP. Implementa una serie di algoritmi per fare in modo di rimediare ai problemi di affidabilità. Tanto è più inaffidabile il canale tanto più è complessa la logica per costruire l'affidabilità, scegliamo un certo trade-off. 

**Rilevare i problemi**: il checksum non è sufficiente; possono avvenire problemi molto più importanti della non integrità del pacchetto. Per esempio possiamo avere perdita di pacchetti che è causata dalla **CONGESTIONE DEI ROUTER**. Inoltre i pacchetti per esempio possono essere duplicati oppure consegnati non in ordine (strettamente legato al paradigma di comunicazione che per tcp è orientato allo scambio di flusso di dati). 

La consegna fuori ordine per TCP è un problema di affidabilità perchè compromette l'intero stream di dati. TCP non viene usato solo per questo motivo da alcune persone.

**Cenni di affidabilità di TCP**
Vogliamo rilevare e rimediare agli errori.
Se vogliamo garantire affidabilità quando perdiamo pacchetti dobbiamo avere acknowledgment. Ogni trasmissione che è andata a buon fine viene notificata dall'host ricevente. Se l'acknowledgment non viene ricevuto entro un certo tempo il mittente fa scadere un timeout e quindi ritrasmette il dato principale. Questo è trasparente all'applicazione, il sistema operativo memorizza il pacchetto una volta solo.

Se un destinatario riceve un pacchetto corrotto semplicemente lo scarta. Esiste il negative acknowlegement che manda un messaggio al ricevente dicendo che il package è corrotto. 

CIò non esiste in TCP perchè per chi lo ha progettato ha pensato che la corruzione dei pacchetti già esiste in H2N quindi ha pensato fosse ridondante. La probabilità che il checksum fallisca per problemi di propagazione è minima.

Anche gli acknowledgment possono essere persi. Il mittente potrebbe re inviare il pacchetto inutilmente. Questo è il motivo per cui abbiamo pacchetti duplicati e quindi va gestito.

Un protocollo si dice orientato alla connessione se ha 3 fasi:
- apertura della connessione
- utilizzo della comunicazione scambio dati
- chiusura della connnessione

Possiamo inviare un numero infinito di byte in TCP, non abbiamo limiti nei dati trasmessi. Deve esistere una logica chiamata **segmentazione** -> ilprocesso di trasformare un flusso di dati infinito a pacchetti piccoli. Il pacchetto piccolo in TCP viene detto **segmento**, è l'output del processo di segmentazione. Ciascun segmento viene incluso in un pacchetto IP. Ciascun datagramma a livello H2N potrebbe essere ulteriormente frammentato. 

Anche l'apertura e la chiusura devono essere affidabili. Uno è un protocollo di apertura/chiusura affidabile che costa molto e l'altro è inaffidabile.

Nel protocollo tcp viene introdotto un sequence number. Ogni segmento viene associato ad un numero. Serve per identificare e scartare i segmenti dulicati e riordinare i segmenti prima di inoltrarli in modo tale da non aver dati disordinati all'invio.

A livello di SO abbiamo un astrazione più complessa. Esiste anche a livello destinatario.
A sinistra c'è cosa si crede di essere TCP a destra c'è l'implementazione effettiva slide 17

In tcp la logica di trasferimento segue il trasferimetno con buffer; le logiche vengono gestite attraverso buffer di memoria che verranno successivamente gestiti dall'SO. 

La logica di controllo tra mittente e destinatario viene legato ad un controllo di flusso. È la regolamentazione del tasso di invio grazie a segnalazioni esplicite tra destinatario e mittente. Il controllo di congestione invece è legato ad una logica di un qualcosa che non va all'interno della rete (es i router droppano un sacco di pacchetti, la rete è congestionata). Diminuire il troughput dei dati scambiati per diminuire il carico sulla rete (congestione) e per diminuire il carico sulle entità comunicanti (flusso).

Le connessioni TCP sono full duplex; quando un client stabilisce una connessione col server possono scambiare informazione bidirezionalmente.

TCP non è pensato per ridurre la latenza, pensa a ottimizzare il throughput. Non ci garantisce una disponibilità di banda, cerca di ottenere il massimo con quello che c'è e se non abbiamo risorse sufficienti non lo possiamo sapere. Non abbiamo inoltre un multicast affidabile, è strettamente unica; cliente-server, utente all'altro.

Abbiamo vari problemi da affrontare a livello TCP:
- Eterogeneità degli host e dei processi in comunicazione
- Eterogeneità dei tempi di trasmissione. I tempi di trasmissione hanno un impatto sulla latenza quindi viene influenzato il timeout.
- Possibilità di avere un host destinatario con capacità computazionale e memoria diversa dall'host mittente.
- Possibilità di avere reti molto diverse.

**Segmento TCP**
L'header del segmento TCP è formato da:

argomento importante
**Apertura della connessione**
Nel protocollo TCP l'apertura della connessione è l'unica fase in cui ci interessa avere un modello client-server.
- Client: Inizia la connessione
- Server: Dev'essere già attivo, sta in attesa e viene contattato dal client.

In questa fase vengono inizializzate delle variabili per verificare il corretto funzionamento della connessione.
- Numeri di sequenza dei segmenti. Non partono da zero ma vengono inizializzati in modo autonomo
- Informazioni per la gestione del buffer...

**Three way handshaking**
Il protocollo di apertura della connessione TCP. Si chiama three way perchè ci saranno 3 interazioni tra client e server.

Il client invia al server un segmento di controllo con SYN = 1 e specifica nello stesso segmento il numero iniziale di sequenza.

Il server nella risposta non manda dati dato che SYN = 1 (che se è non può inviare dati, il payload è vuoto). Come sequence number imposta il suo sequence number e lo usa per contare il numero di pacchetti che il server invierà al client. Il server imposta anche il bit ACK ad 1. Abbiamo finito la fase di inizializzazione.

Il client invia di nuovo un altro segmento che serve per l'affidabilità della connessione. Segnala al server di aver ricevuto correttamente il pacchetto SYN/ACK. Il sequence number è uguale a client_sequence_number + 1 in modo da identificare il fatto che non sia un pacchetto duplicato. L'ack invece è uguale al server sequence number + 1.
In questa fase la connessione È GIA APERTA, QUINDI SI POSSONO GIÀ INVIARE DATI.

Non abbiamo un unico valore globale perchè dato che la comunicazione è full-duplex se entrambe le parti devono comunicare è molto più difficile determinare chi deve incrementare l contatore per primo.

**Campi Base**
- MRW: Comunica la dimensione del buffer dell'entità che sta inviando quel messaggio. È inserito nel campo window size.
- MSS: Campo opzionale, serve per capire la massima dimensione del segmento. Va scelto in base all'MSS.

**Chiusura connessione**
Per chiudere una connessione TCP ci sono due protocolli.
- **Chiusura polite**: ha il campo FIN.
- **Chiusura inaffidabile**: Possiamo chiudere la connessione mandando un singolo segmento, chiamato segnale di RST. Usiamo questo protocollo per due motivi; quando non abbiamo tempo per la chiusura polite e per segnalare incomprensioni nel livello applicativo.
argomento importante

**Affidabilità del protocollo TCP**

**Scenari di ritrasmissione**
Una volta che il pacchetto è stato inviato n volte allora la connessione non si ritiene più utilizzabile.

**Come stimare il time-out**

