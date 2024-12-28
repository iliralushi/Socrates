**Tecnologie per Interconnessioni**
La configurazione dell'infrastruttura fisica viene detta **cablaggio** della rete. 
- Un **mezzo trasmissivo** è un mezzo fisico che abilita la comunicazione tra host.
- Abbiamo **mezzi guidati** come doppini intrecciati, cavi coassiali e fibre ottiche.
- Abbiamo **mezzi ad onda libera** come canali radio terrestri e satellitari.

**Tipologie di Trasmissioni**
In base alle tecnologie usate abbiamo due tipi di trasmissioni:
- Nella **trasmissione analogica** il mezzo fisico può richiedere l'utilizzo di **sistemi di modulazione del segnale** come per i segnali radio o cablaggi a bassa velocità.
- I sistemi cablati di alta qualità permettono la **trasmissione nativa digitale**.

**Tipologie di Canali**
In base alle tecnologie usate possiamo distinguere due tipi di canali trasmissivi:
- **Canale Condiviso**: Gli host devono usare lo stesso canale. Non possono trasmettere contemporaneamente.
- **Canale Dedicato**: Due host possono comunicare tramite un collegamento dedicato a loro. È alla base del collegamento **full-duplex**. 

**Doppino Intrecciato**
È il più economico ed è usato per creare il **cablaggio** delle reti da più di 100 anni. Consiste in due fili di rame isolati singolarmente e poi intrecciati. I fili vengono intrecciati per ridurre i **disturbi EM** coi doppini posti nelle vicinanze.

**Cavo di Rete**
Una serie di doppini isolati elettricamente. Consente la trasmissione digitale di segnali da media distanza.

**Cavo Coassiale**
Un cavo utilizzato per trasmettere segnali elettrici in alta qualità. Abbiamo **diversi livelli di isolamento** tra cavi per evitare interferenze. In diverse varianti è usato sia per trasmissioni analogiche che digitali - audio/video.

**Fibra Ottica**
Un cavo realizzato in materiale trasparente, tipicamente fatto in vetro ma al giorno d'oggi anche in plastica. Trasporta i dati come **impulsi di luce**. Presentano:
- Alta **insensibilità** al disturbo EM.
- Bassa **riduzione** del segnale.
- Ideale per i collegamenti a lunga distanza data la sua sicurezza.
- Non ideale per i collegamenti a corta distanza dato il suo costo.

**Mezzi ad Onda Libera (Wireless)**
In questo tipo di tecnologia il segnale viene trasportato sotto spettro EM. Non ci sono cavi fisici, è bidirezionale e i problemi di **propagazione del segnale** son dovuti sia dall'ambiente che dalla infrastruttura.

**Tecnologie Wireless**
- **Micro-Onde**: Fino a canali a 45 Mbps.
- **WLAN**: Supporta fino a centinaia di Mbps.
- **Wide-Area**: GSM, UMTS, LTE, FWA.
- **Satellite**: Canali fino a 50 Mbps, 200ms di ritardo.

**Canali Radio Terrestri**
Trasportano segnali attraverso onde EM. Non necessitano di cavi, possono trasportare il segnale a lunghe distanze. 

**Canali Radio Satellitari**
Per abilitare la comunicazione satellitare serve porre due o più trasmettitori/ricevitori sulla Terra. Vengono utilizzati per le **reti telefoniche** intercontinental e per alcune **dorsali di Internet**, inoltre supportano centinaia di Mbit/s. Abbiamo una **latenza molto alta**, essa raggiunge i 200ms, ciò succede per l'enorme distanza dalla Terra allo Spazio.

**Cablaggio**
- Ne fan parte le **prese di rete** usate dall'utente per collegare i dispositivi, i **cavi** in rame o fibra ottica, i **dispositivi di commutazione** come switch, hub e bridge, gli **armadi/rack** in cui sono installati i dispositivi di commutazione, i **locali tecnici** in cui sono collocati i rack e i **connettori** che si trovano nelle estremità del cavo.

**Cablaggio Strutturato**
Il cablaggio strutturato consiste nella **metodologia del progetto e realizzazione** dei sistemi di telecomunicazione. Il suo scopo è fornire ad un infrastruttura un cablaggio universale ed unificato.

**Necessità del Cablaggio Strutturato**
È necessario a causa dell'aumento della complessità delle reti e impianti telefonici.
- In passato non si aveva uno standard nel cablaggio, quindi avevamo un sistema dedicato alla voce, uno ai dati ed uno al video, diversificando ogni sistema. Al giorno d'oggi questo causerebbe l'impossibilità di mantenere una tale infrastruttura.

**Cablaggio Strutturato: Topologie**
Tutti gli standard derivano da una **struttura di base "a stella"**. A partire da questa struttura abbiamo diverse topologie di cablaggio strutturato:
- **Topologia a Stella**: Ogni dispositivo ha un collegamento **indipendente** dall'altro, quindi dedicato specialmente ad una coppia di dispositivi. Ogni collegamento è **isolato**.
- **Topologia a Bus**: È formato da un cavo unico con le estremità libere. Viene condiviso da tutti i dispositivi attivi.
- **Topologia ad Anello**: Funziona similmente alla topologia a bus, l'unica differenza sta nelle estremità, qui vengono intrecciate in modo da formare un anello.

![](Cablaggio-Strutturato.jpg)