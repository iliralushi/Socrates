***ovviamente da mettere apposto xd***

**Datagram IPV4**
Tutto il traffico di internet consiste in pacchetti, ciascun pacchetto è lungo fino a 64kb.
Il pacchetto è allineato a 4 byte:
- Vers: primi 4 bit, questo campo identifica la versione del protocollo.
- Hlen: questo campo contiene la dimensione dell'header. Esprime il numero di word (parole) dell'header. Il campo sarà grande hlen * 4 byte. L'header ip è grande 20 byte in ipv4.
- Type of service (service type): è un intero byte, son dei tipi di flag che comunicano il tipo di priorità che il pacchetto che stiamo trasferendo ha (de iure). Adesso il campo serve per gestire delle classi di traffico.
- Total length: Sono 16 bit, questo campo definisce il numero di **byte** che può contenere un pacchetto. Se abbiamo 16 bit riusciamo a rappresentare un massimo di 64kb, il dato detto precedentemente.
- Identification: È un campo che serve ad identificare i pacchetti ip.
- Flags: Sono legati al controllo della frammentazione (flag to not fragment).
- Fragmented offset: Identificativo che stabilisce in quale posizione è il singolo frammento all'interno di uno stesso identificativo. Il campo si chiama offset perchè inseriamo l'offset del frammento del pacchetto intero, è una posizione. Ogni frammento ha un header ip indipendente.
- Time to live: Non esprime un tempo ma esprime quanti router potrà ancora attraversare il pacchetto. Quando un host h1 invia un pacchetto imposta un campo ttl iniziale dove il valore dipende dal sistema operativo. Ogni volta che il pacchetto ip arriva ad un router il router decrementa di 1 il ttl, se il nuovo valore del ttl = 0 allora il pacchetto vien droppato altrimenti viene inoltrato. Se un pacchetto percorre un ciclo/percorre un percorso molto lungo questo campo permette di droppare il pacchetto in questo caso. La logica di ttl può essere legata anche a logiche di segnalazione (pacchetti di segnalazione vedremo + avanti)
- Protocol: Questo è un campo che ci da l'informazione sul protocollo IP utilizzato nel payload.
- Header checksum: Questo è un campo che è legato alla logica di integrità. Serve quindi a verificare l'integrità dell'header del pacchetto. Non garantiamo quindi l'integrità sui dati, un pacchetto IP può avere come un payload modificato mentre è in transito. Se abbiamo NATting allora il checksum va ricalcolato altrimenti da errore di integrità.
- Source ip address (32 bit): 
- Destination ip address (32 bit):

Parliamo di frammentazione quando spezziamo un pacchetto IP in tanti pacchetti H2N. Se il campo fragmented offset è presente indica il numero di pacchetti H2N in cui è stato suddiviso il nostro pacchetto originale.

**Vari scenari di frammentazione**
- Frammentazione locale: Abbiamo un host che crea un pacchetto grande. Vuole trasportarlo al livello inferiore di trasporto. Arriva al livello IP e deve frammentare il pacchetto in tanti sottopacchetti (dipende dalla dimensione dell'MTU) prima di arrivare al livello H2N.
- Comunicazione di rete: Abbiamo una rete composta da tante reti locali diverse. Ciascuna rete locale può avere un MTU differente. C'è un problema di frammentazione lato router se l'MTU di ingresso è maggiore dell'MTU in uscita (pacchetto di 1.5kb vs mtu di 600byte). Aggiungiamo complessità sul router perchè deve gestire la frammentazione. Chi gestisce la frammentazione è l'host finale. Al giorno d'oggi sia in IPV4 sia in IPV6 h1 invia un pacchetto troppo grosso e il router invece di frammentare il pacchetto manda un flag all'host dicendo che non può frammentarlo, quindi il pacchetto viene rinviato all'host e il pacchetto verrà frammentato localmente.

Path MTU discovery = min(mtu) - il valore minimo di MTU di tutte le reti che attraverseremo.