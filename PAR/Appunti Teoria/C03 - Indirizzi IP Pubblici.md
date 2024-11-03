***ovviamente da mettere apposto xd***

**Indirizzi IP pubblici**
Un indirizzo IP pubblico è un indirizzo IP che possiamo utilizzare nella rete globale, quindi un indirizzo che possiamo utilizzare per comunicare direttamente. 

La IANA (Internet assigned numbers authority) è un organizzazione che si occupa di assegnare gli indirizzi IP. Serve un organizzazione centralizzata perchè gli indirizzi IP sono relativamente pochi, quindi per non causare conflitto abbiamo un organizzazione centralizzata che risolve questo problema.

La ICAAN gestisce l'assegnazione degli indirizzi IP e non solo. Non ci affidiamo direttamente a IANA/ICAAN ma a delle organizzazioni intermedie; ne abbiamo una per ogni continente. 

In Europa abbiamo RIPE.
Per comunicare su internet noi non comunichiamo direttamente con RIPE ma con un ISP (internet service provider). ICAAN si occupa di gestire tutti gli indirizzi IP; RIPE ha un certo range di indirizzi e a sua volta gli ISP prenderanno un altro numero di indirizzi. Dipende dalla grandezza dell'ISP, può essere locale come può essere mondiale.

GARR comunica con RIPE ed è un organizzazione italiana che riceve indirizzi da RIPE e gestisce le reti/indirizzi IP dedicati alla ricerca/università. Unimore per esempio ha come IP 155.185.0.0/16.

**Indirizzi privati (non routable)**
Gli indirizzi privati sono gli indirizzi che non possiamo esporre pubblicamente, quindi usare globalmente.

La funzionalità di NAT è una funzionalità tale per cui il router di bordo di questa rete ha disposizione di indirizzi IP e ogni volta che questo router vuole comunicare con l'esterno si occupa di sostituire i pacchetti IP da privati a pubblici. Fa in modo di non far andare mai l'IP privato sulla rete globale, viene quindi sostituito da un altro IP pubblico. Per gestire correttamente il multiplexing tra tanti host dobbiamo sfruttare il livello 4. Il NAT è un protocollo di livello 3 di base, nel suo uso più comune è di livello 3 e 4.

Un router normale inoltra i pacchetti così come sono se non siamo in presenza di NATting. Presi due host uno con indirizzo 1.1.1.1 e uno con 2.2.2.2 e un router che a destra avrà 1.1.1.254 e a sinistra 2.2.2.254. A destra l'indirizzo di sorgente sarà 1.1.1.1 e di destinazione 2.2.2.2. Ciò avverrà anche nell'altra metà.

Il NATting è positivo perchè con un IP privato il resto del mondo non può accedere.

Quando configuriamo una rete con IP privati dobbiamo usare un indirizzo IP non routable. Abbiamo un intervallo di indirizzi per classi:
- 10.0.0.0 - 10.255.255.255 indirizzi di classe A (10.0.0.0/8)
- 172.16.0.0 - 172.31.255.255 indirizzi di classe B (172.16.0.0/12)
- 192.168.0.0 - 192.168.255.255 indirizzi di classe C (192.168.0.0/16)

Gli indirizzi privati possono essere solo all'interno di questo range.

La rete locale è la rete H2N, la rete privata è una rete che indica una rete non routable ma anche loro possono avere tante reti LAN, quindi non hanno bisogno di comunicare con NATting che lo abbiamo una volta che usciamo dalla rete locale.