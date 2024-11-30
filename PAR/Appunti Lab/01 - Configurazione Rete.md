**Prima Configurazione di Rete**
Dobbiamo creare una rete. Essa sarà composta da due host collegati da uno switch.
- Il nostro obiettivo è fare comunicare gli host configurando hostname ed indirizzi IP usati in figura.

**Tipologia di Cavi**
- **Cavo Incrociato**: Usato per la connessione dei dispositivi dello stesso livello come host-host oppure switch-switch.
- **Cavo Dritto**: Usato per connessione di dispositivi di livello diverso, quindi host-switch.

**Step per Configurazione della Rete**
Bisogna eseguire quattro step per configurare il funzionamento della rete:
1) Impostazione del proprio hostname.
2) Impostazione della corrispondenza hostname/indirizzo IP degli altri host della rete.
3) Impostazione del proprio indirizzo IP.
4) Test di connettività.

**Hostname**
L'hostname è il nome identificativo di un host all'interno della rete. Possiamo assegnarlo in due modi diversi:
- **Temporaneamente**: Si può assegnare attraverso il comando `hostname <nome>` e non verrà salvato una volta spenta la macchina.
- **Permanentemente**: Si può modificare il file contenuto nel path `/etc/hostname.`

All'avvio di un host Marionnet configura automaticamente l'hostname, verrà sovrascritta quindi ogni modifica effettuata.




6) Configurare hostname: si modifica il file /etc/hostname per mettere l'hostname
7) Si modifica /etc/hosts assegnando i nomi ad ogni dispositivo
8) Si configura la rete modificando il file /etc/network/interfaces. iface (nomeinterfaccia) eth0 (tipoconfigurazione) inet/ipv4 (keyword) static. Riga sotto address/netmask/network/broadcast/gateaway e indirizzo ip. Sopra va messa una direttiva auto (nomerete) per avviarlo all'avvio del dispositivo.
9) Ifup/ifdown (nomeinterfaccia) per spegnere ed accendere e checkare. -a per agire su tutte le interfacce.
10) Per vedere se funziona usare comando ping eth0 ip