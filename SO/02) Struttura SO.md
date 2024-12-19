**Stratificazione SO**
Un SO moderno è concepito **a strati**. Gli strati formano uno **stack software**, ogni strato è implementato sfruttando le funzioni esclusivamente degli strati inferiori.

![](../../Images/Stack-Software.png)

**Contenuto Strato Software**
Uno strato contiene le **funzionalità** e le **strutture dati** idonee per la creazione delle funzionalità degli strati superiori. Si possono implementare in due modi:
- **Hardware**: Chip ROM, EEPROM...
- **Software**: Librerie statiche o dinamiche.

**Implementazione Strato Funzionale**
Le funzionalità dello strato `m` fanno uso delle funzionalità dello strato `m-1.` In questo caso `fmj` è una funzione pubblica, quindi usabile dallo strato `m+1.` `fmj` può implementare delle funzioni private di supporto visibili **solo** allo strato `m` che possono usare le funzionalità di `m-1.` Il ciclo si ripete per gli **strati superiori**.

![](../../Images/Strato-Funzionale.png)
![](../../Images/Strato-Funzionale2.png)

**Vantaggi: Modularità**
- **Sviluppo**: Estremamente comodo; il primo strato viene progettato senza considerare il sistema, gli altri strati si appoggiano a quello precedente.
- **Debugging**: Se i `m-1` strati sono corretti ciò vuol dire che l'errore risiede nello strato `m.`

**Svantaggi**
- **Definizione Strati**: Ci sono molti fattori da considerare, rendendo l'implementazione molto complicata in un SO moderno. Un esempio è il meccanismo dello **swapping**. Il sistema del gestore della memoria ha bisogno dei **driver del disco** per crearlo, quindi vengono messi in uno strato **inferiore**.
- **Efficienza**: Al giorno d'oggi trascurabile. Più strati comportano ritardo dato che c'è una sequenza più lunga di funzioni.

**SO Basato su Macro Kernel**
L'insieme dei servizi esegue in **kernel mode** mentre l'insieme delle applicazioni esegue in **user mode**. Le funzionalità essenziali del kernel risiedono in un immagine, viene eseguita durante il **boot** del sistema.

![](../../Images/Macro-Kernel.png)

**Pros**
- L'esecuzione dei servizi è la più **veloce possibile**. (User -> kernel & kernel -> user)

**Cons**
- Molto **fragile**. Un crash del kernel blocca la CPU.
- Dimensione del kernel **enorme**.

**Code Base UNIX**
Con l'aumentare dei servizi la dimensione del kernel dei sistemi UNIX è cresciuta notevolmente. Esistono due soluzioni:
- L'uso di **moduli caricabili**.
- Creare un SO basato su **micro kernel**.

**Moduli Caricabili**
Sono dei file oggetto contenenti **funzionalità del kernel**. Sono disattivabili a tempo di esecuzione.

**Pros**
- La dimensione base del kernel diminuisce **notevolmente**.
- Un crash di un modulo non impalla il sistema.

**Cons**
- Lieve perdita di prestazioni dovuta alla dis/attivazione dei moduli.

**SO Basato su Micro Kernel**
Il kernel esegue soltanto i servizi **essenziali**. Il resto viene eseguito sotto forma di server applicativo che comunicano tra di loro tramite un **sistema a messaggi**.
- **Kernel**.