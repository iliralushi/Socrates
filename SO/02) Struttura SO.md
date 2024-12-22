**Stratificazione SO**
Al giorno d'oggi un SO è progettato **per strati**. Ogni strato viene progettato sfruttando le funzionalità dello strato **precedente**. L'insieme di strati forma uno **stack software**.

![](../../Images/Stack-Software.png)

**Contenuto Strato**
Contiene l'implementazione di tutte le **funzionalità e strutture dati** necessarie per creare lo strato di livello superiore. Esse possono essere implementate in due modi:
- **Software**:  Uso di librerie statiche/dinamiche.
- **Hardware**: Uso di chip ROM/EEPROM.

**Generico Strato Funzionale**
1) Le funzionalità dello strato `m` fanno uso delle funzionalità dello strato `m-1.` Ogni strato ha una parte **pubblica** ed una **privata**. Qui lo strato `m+1` può sfruttare la funzione `fmj.`
2) Lo strato `m` definisce funzioni **private di supporto** che possono sfruttare le funzioni pubbliche dello strato `m-1` ma non sono visibili allo strato superiore.
3) I primi due step vengono riapplicati fino all'ultimo strato: le funzioni dello strato `m+1` vengono implementate usando le funzionalità **pubbliche** dello strato `m.` 


![](../../Images/Strato-Funzionale.png)
![](../../Images/Strato-Funzionale2.png)

**Vantaggi: Modularità**
- **Nello Sviluppo**: Nel primo strato vengono implementate le funzionalità per **interfacciarsi con l'hardware** ed il secondo strato si appoggia alle funzionalità del primo.
- **Nel Debugging**: Dato che è uno **stack di strati** se i primi `m-1` sono funzionanti vuol dire che il problema risiede nello strato `m.`

**Svantaggi: Definizione Strati**
È difficile definire l'inizio e la fine di uno strato.
- **ES**: Il sistema di gestione della memoria ha bisogno del driver del disco per implementare il processo di **swapping**. Lo strato dei driver del disco **va collocato al di sotto** dello strato della memoria. In un SO moderno si hanno **moltissimi vincoli** di questo genere.

**Svantaggi: Efficienza**
L'uso di più strati comporta un **ritardo maggiore** nel servizio per via delle cascate di funzioni più lunghe. Con l'aumento della potenza di calcolo al giorno d'oggi è trascurabile.

**Macro Kernel SO**
I servizi vengono eseguiti in **kernel mode**, le applicazioni in **user mode**. Le funzionalità essenziali del kernel vengono messe in un **immagine** che viene eseguita al **boot** del sistema.

![](../../Images/Macro-Kernel.png)

**Vantaggi e Svantaggi**
- **Vantaggi**: Esecuzione servizi **più veloce possibile** (user -> kernel / kernel -> user).
- **Svantaggi**: Kernel **fragile**, se crasha **si blocca la CPU**, inoltre ha dimensione **enorme**, quindi le istruzioni da eseguire potrebbero non entrare in **CPU cache**, si ha un calo di prestazioni **significante**.

**Dimensione Codebase UNIX**
Con l'aumentare dei servizi è aumentata anche la **dimensione del kernel**.
- Per allegerirne il peso si possono usare **moduli caricabili** o SO basati su **micro kernel**.

**Moduli Caricabili**
**File oggetto** contententi funzionalità del kernel. Sono (dis)attivabili a **tempo di esecuzione**.
- **Vantaggi**: La codebase del kernel diminuisce **notevolmente**, il crash di un modulo non blocca il sistema.
- **Svantaggi**: Si ha una lieve perdita di prestazioni dovuta alla (dis)attivazione dei moduli.

**Micro Kernel SO**
In kernel mode vengono eseguiti solo i **servizi essenziali**. Il resto viene eseguito sotto forma di **server applicativo** in user mode. Essi comunicano tramite un **sistema a messaggi**.
- **Kernel**: Mezzo di comunicazione per i **server**, scheduler CPU, gestore della memoria.
- **Applicazioni**: Gestore del file system, tutti i driver dei vari dispositivi.

**Vantaggi e Svantaggi**
- **Vantaggi**: Facilità di estensione; nuovo servizio -> nuovo server, il kernel non viene toccato. Il kernel ha dimensioni **minori** ed è CPU cache friendly. L'SO è **robusto**, se muore un server **si crea un'altra istanza** mentre l'SO continua ad eseguire.
- **Svantaggi**: Meno performante di un SO con architettura **macro kernel**.

![](../../Images/Micro-Kernel.png)

**Hybrid Kernel SO**
Simile al micro kernel. I driver dei dispositivi vengono eseguiti in **kernel mode**.

![](../../Images/Hybrid-Kernel.png)
![](../../Images/MVMVH.png)