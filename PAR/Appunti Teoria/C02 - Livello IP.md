***ovviamente da mettere apposto xd e finire***

Possiamo usare il protocollo IP per identificare reti intere invece di singoli indirizzi

3 classi utilizzabili per indirizzo di host
- Classe a = 1 byte di netid
- Classe b = 2 byte di netid
- Classe c = 3 byte di netid

La notazione classless (CIDR, classless inter-domain routing) è la notazione più usata al giorno d'oggi. Usiamo la solita notazione dotted e lo separiamo con uno slash ed un numero per indicare quanto è grande il nostro netid. Elimina l'usaggio delle classi prestabilite e forza un tipo di classe. Il valore n è la netmask dell'indirizzo di rete (netmask in formato compresso).

Esistono indirizzi IP speciali che non possiamo assegnare altri sono indirizzi che possiamo assegnare per alcuni ruoli specifici.
- Network address: un indirizzo ip in cui il campo relativo all'hostid è uguale a 0. Sono tutti indirizzi che non possiamo assegnare ad host perchè sono tutti gli indirizzi dedicati a rappresentare delle reti già assegnate.
- Directed broadcast address: è un indirizzo di broadcast che rappresenta l'indirizzo di brodacast per una specifico indirizzo ip. Mettiamo tutti i bit a 1 dell'hostid. Permette il broadcast a tutti gli host di una certa rete, anche non solo fisica.
- Limited broadcast address: tutti i bit sono uguali ad uno. Subnet mask. Rimane sempre nella rete fisica, non inoltreranno mai un packet broadcast fatto in questa maniera. 255.255.255.255, possiamo lavorare solo sulla singola rete fisica.
- Nessun indirizzo IP: tutti i bit sono uguali a zero (0.0.0.0). Indirizzo IP che ha come valore null.
- Loopback address (localhost): 127.0.0.1, è un indirizzo software che serve per relegare se stessi. Questo non è un indirizzo ma è una rete.

La formula per calcolare il numero di indirizzi possibili è $2$ ^ $(num bit - bit netmask)$

Il subnetting è la scomposizione di una rete in più reti. A partire da un indirizzo ip ne dobbiamo creare un altro a patto che stia all'interno della rete. Per creare un altra rete da una rete principale assegnamo due subnetid differenti. Con due reti una sarà a 0 e l'altro a 1. Ogni volta che spezzo la rete perdo qualche indirizzo assegnato.

In una suddivisione di reti non bisogna mai creare sovrapposizioni.

**Routing**
Ogni host e ogni router hanno una tabella di routing. Ogni nodo della rete ip deve sapere come aggiungere qualsiasi destinazione di quella rete ip. Il percorso dei pacchetti viene selezionato hop-by-hop. La tabella è composta da due colonne; la destinazione che dice dove andrà a finire i nostri dati ed il metodo utilizzato (h2n, ip...) che stabilisce il protocollo utilizzato.

4 componenti fondamentali nell'architettura di un router:
- Una porta di ingresso; c'è una parte dove arriva il cavo fisico, una parte che analizza il frame ethernet e una coda (buffer) dove il pacchetto viene memorizzato in attesa dello switcher fino a poi inviare il pacchetto. Il riempimento di questa coda può causare dei problemi; la memoria può riempirsi, finisce la memoria e avremo una perdita di pacchetti. Oltre al caso estremo se un pacchetto rimane troppo nella coda avremo comunque un aumento della latenza, quindi dobbiamo avere una logica di routing efficiente.
- Un commutator; la logica dell'invio del pacchetto. Possiamo avere una comunicazione di tipo arp dove tutto è condiviso, tipo bus, oppure di tipo mesh dove possiamo far comunicare una qualsiasi porta di ingresso con una qualsiasi porta di uscita.
- Un processore di routing che decide come azionare questa logica. Abbiamo una coda in uscita che si riempie quando la velocità con cui inseriamo i pacchetti è più lenta del numero di pacchetti in uscita. Quando una coda si riempie diciamo che abbiamo una congestione, quindi un riempimento del buffer. La coda è una coda first in first out. 

In teoria le logiche di progettazione del router dovrebbero essere compliant al net neutrality.