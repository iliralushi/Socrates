Il dhcp (dynamic host configuration protocol) è un protocollo che serve per la **configurazione automatica** degli host che si connettono ad una rete.

È un protocollo di livello applicativo (basato su UDP) ma che usiamo quando non abbiamo configurato IP, serve quindi a configurare IP. Dato che non abbiamo il livello IP si fa aiutare dal livello H2N a basso livello dato che comunque si può avere una comunicazione fisica. è fatto per funzionare in un contesto di guido; in una rete DHCP possiamo avere degli host configurati anche staticamente, macchine che non istanziano un client DHCP. 

Protocollo di tipo stateful; c'è un informazione di stato. UDP è stateless.

L'obiettivo del DHCP è di essere in grado di setuppare l'host in modo da comunicare con qualsiasi **altro host su Internet**. Il protocollo DHCP è pensato per configurare gli host non i router.

DHCP è basato su paradigmi **client-server**. Un host esegue il server DHCP poi tutti gli atrli host eseguono il server DHCP. 

Il server mantiene delle policy di rete, vien centralizzata la distributzione delle informazioni, sarà lui a mettere tutte le impostazioni agli altri host. Gli host devono estrarre queste regole automaticamente. Protocollo con architettura scalabile. Funziona su una o più subnet di rete, il server DHCP può mantenere le policy di rete con tantissime reti e riesce a distribuirle sia alle reti locali che alle reti logiche. 

**Policy di Rete**
Le policy sono informazioni di configurazioni, devono riuscire a modellare delle configurazioni per assegnare indirizzi ip/regole di routing etc... Deve esistere un database con tutte le policy che voglio propagare nella rete.
- Queste policy possono essere manuali ((il binding viene definito staticamente nel database) oppure dinamiche (il server DHCP può scegliere a runtime quali informazioni distribuire). Quando si parla di dinamica si parla di tempo di lease; quanto tempo quell'indirizzo ip può essere utilizzato da un certo client. Serve giustamente per no nesaurire tutti gli host disponibili.
- Le policy di rete sono tutte chiave-valore. La chiave identifica univocamente un host. Coppia composta dal netid della subnet + un identificativo dell'hsot.

**Protocolli di Comunicazione**
Il protocollo funziona con dei client che non hanno indirizzo IP. Questa situazione viene gestita utilizzando le porte (UDP). Quando vengono inviate le richieste non vengono scelte porte a caso ma porte a numero basso, abbiamo delle porte quidni anche per le richieste non solo per il server. Le well known ports sono 67 per server e 68 per l'invio da parte di host al server.

**Pacchetto DHCP**
Alcuni parametri:
- **Op**: Campo particolare che identifica il messaggio, ha valore 1 per i messaggi client server e 2 per i messaggi server client
- **Xid**: Transaction ID, scelto casualmente dal client. Serve per questioni di sicurezza e per gestire lo stato. Viene randomizzato per race condition, una gara tra server malevoli e server benevoli. Perchè funziona? Statisticamente il server benevolo è connesso via cavo quindi c'è molto meno latenza rispetto ad un server malevolo che sarà connesso via wifi maggior parte delle volte. L'altra alternativa sarebbe indovinare il transaction ID e qui entra in gioco la randomizzazione.
- Altri campi sono tutte informazioni da dare come ciaddr (client ip server) yiaddr (nostro ip server) siaddr (next server ip) chaddr (client hw addr)
- **Options**: Sono campi aggiuntivi/facoltativi (server DNS, regole di routing, informazioni aggiuntive che non sono nei campi principali). Ogni opzione è identificata da un numero intero.

**Tipo di pacchetti da client a server**
- **DHCPDISCOVER**: È prevista come prima informazione inviata dal client al server DHCP tramite broadcast, chiede se esiste un server DHCP.
- **DHCPREQUEST**: È un messaggio che richiede una conferma. Il client ha già un indirizzo/è una rete vecchia quindi già memorizzata.
- **DHCPDECLINE**: Il client rifiuta un indirizzo proposto da un server già allocato.
- **DHCPRELEASE**: Pacchetto che client invia quando non vuole più utilizzare un indirizzo già assegnato e gli dice che lo può usare per altro.
- **DHCPINFORM**: Pacchetto che il client può inviare al server se vuole ottenere informazioni considerate aggiutnive rispetto all'indirizzo IP.

**Tipo di pacchetti da server a client**
- **DHCPOFFER**: Offre un indirizzo DHCP ad un client.
- **DHCPACK**: Acknowledgment positivo, accettazione dell'indirizzo proposto dal client e invio delle informazioni di configurazione.
- **DHCPNAK**: Acknowledgment negativo, rifituo dell'indirizzo proposto dal client.

**DHCP server con DNSMASK**
Il software più popolare che riguardi reti di piccole o medio/piccole dimensioni.

**DHCP relay agent**
Relay agent = un componente che ha come uruolo quello di inoltrare le informazioni. Il compito di un relay agent è di essere eseguito in tutte le reti locali dove non è presente DHCP. L'alternativa è installare tanti server DHCP.


