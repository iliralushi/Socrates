**Livello IP - Stack TCP/IP**

![](IP.png)

**Internet - Una Rete Globale**
L'obiettivo di Internet è connettere un qualsiasi numero di reti (scalabilità) **eterogenee** ed **indipendenti**.
- **Reti Eterogenee**: Distinzione dei layer nei suoi vari componenti.
- Le comunicazioni sfruttano il **packet switching**. I pacchetti vengono inoltrati con logica stateless, quindi avranno una complessità bassa. **L'intelligenza** viene gestita dagli host.

**Internet - Un Insieme di Nodi e Reti**
I router sono i nodi intermedi, mentre gli host sono i nodi terminali.
- **Indirizzamento Gerarchico**: Suddivisione di reti in più sottoreti.
- Ogni nodo ha **ALMENO** un **indirizzo IP univoco**.
- I nodi uniti formano una **RETE**. Sono identificabili attraverso **blocchi di indirizzi**.

**Internet - In Sintesi**
- **FISICAMENTE**: È un insieme di componenti **eterogenee** come host, router e links. L'eterogeneità è gestita dai protocolli H2N che devono essere compatibili con i protocolli IP anche se possono essere implementati in modo indipendente.
- **LOGICAMENTE**: È una rete suddivisa in più sottoreti tramite **indirizzamento gerarchico**. L'inoltro dei pacchetti avviene coi router tramite **packet switching** con logica **stateless**. I pacchetti sono privi di intelligenza in modo da sfruttare i router al meglio, vengono gestiti dall'host.

**Principi Funzionali di IP**
- **Survivability**: Se esiste un percorso tra host la comunicazione deve poter avvenire.
- **Scopo**: IP rappresenta il layer "ponte" tra il livello H2N e i livelli successivi. Si preoccupa solo di inviare pacchetti in modo efficiente senza fare assunzioni sul contenuto/su chi lo ha inviato.
- **Mancanza di Stato**: L'intelligenza del pacchetto è gestita dagli host. La comunicazione è molto più veloce e aumenta la survivability.
- **Net Neutrality**: Ogni pacchetto viene trattato allo stesso modo.
- **Decentralizzazione**: Le reti sono possedute/gestite da enti diversi.

**Indirizzo IPv4**
Serve per identificare **in modo unico** ogni nodo della rete, essenziale per avere un **servizio di comunicazione universale** come Internet.
- **Esempio**: `130.192.5.189`
- È grande 32 bit (4 byte) e usa la **dotted notation**. Ogni byte rappresenta un campo.
- Solitamente la lunghezza degli indirizzi è **fissa** mentre il suo **spazio di indirizzamento** è gerarchico.

**Componenti Indirizzo IP**
Ogni indirizzo IP è composto dalla coppia `<netid> <hostid>.`
Il **netid** identifica la rete mentre l'**hostid** identifica l'host all'interno della rete.
- La notazione permette di identificare blocchi di indirizzi in modo rapido.
- La grandezza dei campi `<netid> <hostid>` è variabile.

**Assegnazione Indirizzi IP**
Lo **spazio degli indirizzi** viene gestito in blocchi di indirizzi. Una rete gestisce in **modo esclusivo** gli indirizzi con `<netid>` corrispondente a quello della rete.
- Un blocco di indirizzi può essere assegnato ad una rete locale dove gli indirizzi comunicano a livello H2N.
- **Approccio Ricorsivo**: Un blocco di indirizzi può essere assegnato ad una rete logica che contiene più reti logiche/locali.

![](Assegnazione-IP.png)

**Classi di Indirizzi IP**
È uno dei modi per assegnare lo spazio ai campi `<netid> <hostid>.` Si parte dal MSB. Metodo quasi inutilizzato al giorno d'oggi. Le classi sono statiche, c'è bisogno di flessibilità.

``` CashCarti
A: 1 byte netid / 3 byte hostid (0)
B: 2 byte netid / 2 byte hostid (10)
C: 3 byte netid / 1 byte hostid (110)
```

**Classless Inter-Domain Routing (CIDR)**
Suddivisione in **bit** dei campi. Il suffisso `/n` indica i bit del netid. Al contrario delle classi non abbiamo alcun limite grazie all'uso dei **bit** e non **byte**.
- **Esempio**: `a.b.c.d/n.`

**Assegnamento Indirizzi IP**
Gli indirizzi IP sono **logici** e non fisici. Un host deve avere un indirizzo IP per l'identificazione. L'host riconosce il proprio indirizzo tramite due modi:
- **Configurazione Manuale**: L'indirizzo viene configurato permanentemente nell'host in un file dall'amministratore di sistema.
- **Dynamic Host Configuration Protcol (DHCP)**: L'indirizzo viene allocato dinamicamente durante il boot dell'host.

**Indirizzi IP Speciali**
Indirizzi **NON** assegnabili/assegnabili solo in situazioni particolari:
- **Network Address**: Indirizzo con i bit dell'**hostid** pari a zero. Rappresenta il **netid** che è stato assegnato ad una rete, quindi non è assegnabile.
- **Directed Broadcast Address**: Indirizzo con i bit dell'**hostid** pari ad 1. Per una certa rete rappresenta l'indirizzo di broadcast, quindi permette il broadcast a tutti gli host. Non è limitato alla rete fisica ma a tutta la rete logica.
- **Limited Broadcast Address**: Indirizzo con tutti i bit pari ad 1. Permette il broadcast nella rete **fisica**, quindi è limitato dal cablaggio e dall'infrastruttura della rete.
- **Dummy IP**: Indirizzo con tutti i bit pari a zero. Usato per il boot dell'host.
- **Loopback Address (Localhost)**: È un indirizzo di classe A con `<netid>` pari a 127. Viene usato principalmente per comunicare con se stessi.

**Router**
Il router si occupa della survivability della comunicazione. Ha il compito di **instradare un qualsiasi pacchetto** dall'host mittente all'host destinatario basandosi sull'IP destinazione contenuto al suo interno.

**Routing IP**
I pacchetti vengono inoltrati **hop-by-hop**. Conosciamo solo il router successivo a cui verrà inoltrato il pacchetto e non il percorso completo a priori.
- Il routing non è infallibile. Se un router è sovraccarico possiamo avere la perdita di pacchetti (congestione) oppure ci possono essere errori di routing (cicli di rete).

![](Routing-IP.png)

**Inoltro Hop-By-Hop dei Pacchetti IP**
L'host che invia il pacchetto all'esterno della rete **locale** deve decidere il router per inviarlo. Viene detto **first-hop router**. Successivamente anche il router deve scegliere un altro router a cui inoltrare il pacchetto detto **next-hop router**.
- Se il pacchetto percorre troppi router potrebbe essere scartato (TTL).
- Il processo continua fino ad arrivare all'host destinazione. L'ultimo router viene detto **destination router**.

**Problema del Routing**
Il routing non è **affidabile**. Potrebbero esserci troppi router intermedi, quindi il pacchetto può scadere, quindi la sua ricezione non è scontata. Suddividiamo il problema in 2 parti:
1) **IP Forwarding**: Trova il link di uscita per ogni pacchetto all'interno della rete in modo da avvicinarlo alla destinazione.
2) **Protocollo di Routing**: Mantiene le informazioni aggiornate del pacchetto su una tabella.

**IP Forwarding**
Modo in cui un router trasferisce i datagram da un'interfaccia di ingresso ad una in uscita.
Viene effettuato da ogni router. Il **next-hop router** appartiene sempre ad una rete alla quale **il router è collegato a livello H2N**.
- L'indirizzo di destinazione viene estratto dall'header del datagram.
- L'indirizzo di destinazione è usato come **indice** nella **tabella di routing**.

**Caratteristiche IP Forwarding**
- **Indipendenza dal Mittente**: Il next-hop routing non dipende dal mittente del pacchetto o dal cammino svolto. Il router estrae **solo l'indirizzo del destinatario**.
- La tabella di routing deve contenere un next-hop router per ciascuna destinazione.
- Il next-hop router appartiene sempre ad una rete dove il router è **collegato via H2N**.

**Tabella di Routing**
Ogni **host** e ogni **router** hanno una **tabella di routing** composta da un campo destinazione ed un campo metodo. Viene fornito il next-hop per ogni possibile destinazione.
- Il percorso dei pacchetti viene scelto **hop-by-hop**.

![](Routing-Table.png)

Come esempio creo la tabella per **Rete 1** e **Router 2**

| Destinazione | Metodo              |
| ------------ | ------------------- |
| Rete 1       | H2N                 |
| Rete 2       | IP tramite Router 1 |
| Rete 3       | IP tramite Router 1 |

| Destinazione | Metodo              |
| ------------ | ------------------- |
| Rete 1       | IP tramite Router 1 |
| Rete 2       | H2N                 |
| Rete 3       | H2N                 |

**Funzionamento del Router**
Ogni router **estrae l'IP** di destinazione dall'header e consulta la **tabella di routing**:
1) Se l'IP è contenuto in una rete **nota** dove il router **è connesso a livello H2N** il pacchetto viene inoltrato via H2N.
2) Se l'IP è contenuto in una rete **nota** dove il router **NON** è connesso a livello H2N allora si usa il protocollo IP. Nella tabella è contenuto il next-hop router a cui inviare il pacchetto.
3) Se l'indirizzo non appartiene ad alcuna rete nota ma esiste un router di default (**default gateaway**) si invia il pacchetto via IP.
4) Se non rientriamo in nessuna di queste casistiche: **network unreachable**.

**Dimensioni Tabella Routing**
Per risolvere il problema delle dimensioni crescenti delle tabelle di routing si aggrega:
- Le **tecniche di aggregazione** fanno in modo di catturare più reti di destinazione in una:
- Si assegnano gli IP in modo corretto; in una rete locale usiamo indirizzi adiacenti in modo da catturare più reti in una.
- Si usa il subnetting/supernetting e l'indirizzamento gerarchico.

**Protocolli di Routing**
In contesti semplici la tabella di routing può essere costruita **staticamente**/da un protocollo di configurazione tipo DHCP mentre vengono create dinamicamente coi **protocolli di routing**.

**Subnetting e Supernetting**



4 componenti fondamentali nell'architettura di un router:
- Una porta di ingresso; c'è una parte dove arriva il cavo fisico, una parte che analizza il frame ethernet e una coda (buffer) dove il pacchetto viene memorizzato in attesa dello switcher fino a poi inviare il pacchetto. Il riempimento di questa coda può causare dei problemi; la memoria può riempirsi, finisce la memoria e avremo una perdita di pacchetti. Oltre al caso estremo se un pacchetto rimane troppo nella coda avremo comunque un aumento della latenza, quindi dobbiamo avere una logica di routing efficiente.
- Un commutator; la logica dell'invio del pacchetto. Possiamo avere una comunicazione di tipo arp dove tutto è condiviso, tipo bus, oppure di tipo mesh dove possiamo far comunicare una qualsiasi porta di ingresso con una qualsiasi porta di uscita.
- Un processore di routing che decide come azionare questa logica. Abbiamo una coda in uscita che si riempie quando la velocità con cui inseriamo i pacchetti è più lenta del numero di pacchetti in uscita. Quando una coda si riempie diciamo che abbiamo una congestione, quindi un riempimento del buffer. La coda è una coda first in first out. 

In teoria le logiche di progettazione del router dovrebbero essere compliant al net neutrality.

La formula per calcolare il numero di indirizzi possibili è $2$ ^ $(num bit - bit netmask)$

Il subnetting è la scomposizione di una rete in più reti. A partire da un indirizzo ip ne dobbiamo creare un altro a patto che stia all'interno della rete. Per creare un altra rete da una rete principale assegnamo due subnetid differenti. Con due reti una sarà a 0 e l'altro a 1. Ogni volta che spezzo la rete perdo qualche indirizzo assegnato.

In una suddivisione di reti non bisogna mai creare sovrapposizioni.