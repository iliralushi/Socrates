**SO Moderno**
- **Multiprogrammato**: Più applicazioni in esecuzione contemporaneamente.
- **Multiutente**: Più utenti che possono usare il dispositivo contemporaneamente.
- **Time Sharing**: I programmi vengono eseguiti **sequenzialmente** in piccole parti invece che tutti contemporaneamente.

**Eseguibile vs Processo**
- **Programma Eseguibile**: Programma salvato su **storage device**. Contiene il **codice macchina**, alcune aree di memoria e delle funzione utili per il **debugging**. Non può andare in **esecuzione da solo**.
- **Processo**: Rappresentazione del kernel in termini di **strutture dati** e **funzioni di gestione** di un **programma eseguibile in esecuzione**.

**Processo**
- **Strutture Dati**: Rappresentazione di uno **stato interno** e puntatori a **risorse in uso** come aree di memoria o altri programmi.
- **Funzioni di Gestione**: Creazione, terminazione, comunicazione e sincronizzazione dei processi. Esecuzione di un programma eseguibile.

**PID**
Il PID è un **numero univoco** assegnato dal kernel per ogni singolo processo. È rappresentato dal tipo di dato `pid_t` che è un **intero non negativo**.
- Viene assegnato seguendo la formula `next_pid = (prev_pid + 1) % max_pid.`

**Stato Interno Processo**
- **In Esecuzione**: Del codice sta venendo eseguito dalla CPU.
- **Bloccato**: In attesa di un evento, tipicamente ricevuta dati da una periferica.
- **Pronto**: È finito un evento ed il processo è pronto per eseguire la sua esecuzione.
- **Terminato**: Non c'è più codice da eseguire. Vengono **liberate le risorse** ed il processo viene **ucciso**.

![](Automa-Stati.png)

**Linux: In Esecuzione**
Per Linux non c'è alcuna differenza di stato tra **pronto** ed **in esecuzione**. Sono entrambi catturati dallo stato `TASK_RUNNING.`

**Linux: Comunicazione**
Un processo può comunicare con gli altri sfruttando **segnali** come `CTRL+C` che è il segnale di interruzione del processo. Se si chiude un processo mentre sta ricevendo dati potrebbe rimanere in uno **stato inconsistente**. L'attesa è gestita tramite due stati:
- `TASK_INTERRUPTIBLE` e `TASK_UNINTERRUPTIBLE.`

**Linux: Sincronizzazione**
Un processo può **creare altri processi** ed aspettare che essi terminano (`wait()`). Quando è terminato **salva il suo stato di uscita** in una struttura dati, così il processo padre lo riesce a leggere. L'uscita di un processo è divisa in due stati:
- `EXIT_ZOMBIE:` Il processo creato **è morto**, le risorse non son state deallocate. Il processo creatore può leggerne lo stato di uscita.
- `EXIT_DEAD:` Il creante ha letto lo stato di uscita, le risorse possono essere deallocate.

**Linux: Debugging**
- `TASK_STOPPED:` Il processo è stato fermato con CTRL+Z.
- `TASK_TRACED:` Il processo è attualmente tracciato da un debugger.

![](Automa-Stati-Linux.png)

**Descrittore Processo**
Quando viene creato un processo il kernel crea una scrittura dati detta **Process Control Block (PCB)** che contiene informazioni sullo **stato** e puntatori a **risorse prenotate**.
- In Linux il PCB è definito dalla `struct task_struct.`

![](PCB.png)

**Creazione di Processi**
Mentre è in esecuzione un processo può **CLONARE** (**forking**) altri processi. Al termine della clonazione il processo figlio è identico al processo padre.
- Il processo **clonante** è il padre, il processo **clonato** è il figlio.

**fork()**
Nei sistemi UNIX è la **chiamata di sistema** usata per clonare esattamente un processo. In Linux è implementata dalla funzione di servizio `do_fork().` Identifica le risorse da clonare per un singolo processo, clona le risorse con la funzione `copy_process().`
- `do_fork()` è definita nel path `$LINUX/kernel/fork.c`

![](Fork.png)

**Dopo la Clonazione**
Al termine di `fork()` sia il processo padre che quello figlio continueranno l'esecuzione dell'istruzione dalla prossima. Distinguiamo i processi grazie a ciò che returna `fork().`
- **Processo Padre**: Ritorna il PID del processo figlio.
- **Processo Figlio**: Ritorna `0.`
- **Errore**: Ritorna `-1.`

**Organizzazione Processi in SO UNIX**
L'organizzazione dei processi in UNIX è **ad albero**. 
1) `init` è un qualsiasi processo iniziale creato **a mano dal kernel**, ha sempre 1 come PID.
2) `init` fa partire i servizi forniti dal PC tramite `fork` ed `exec` come `getty` che è il **gestore dei login** su console.
3) Al termine del login `getty` fa partire una shell (**BASH**).

![](Albero-Processi.png)

**Esecuzione di Programmi Eseguibili**
`fork()` non carica altri programmi. Il caricamento è gestito dalla chiamata di sistema `execve()` che sostituisce efficientemente le aree codice/dati di un processo.

**Usi Buoni del Forking**
Solitamente si usa per creare comandi **composti** in pipeline.
- `find . -name \*.c | wc -l`

**Usi Cattivi del Forking**
Una **fork bomb** è un processo speciale che riproduce processi figli all'infinito. **È ricorsivo** quindi anche i figli creano figli. Meccanismo utilizzato per DDoS.
- Il kernel impone un **tetto massimo** di processi creabili. Una volta raggiunto non si possono creare altri processi, a quel punto la macchina sarà inutilizzabile, i processi consumano risorse, quindi rallentano.

![](Forkbomb.png)

**Terminazione Processi**
La chiamata di sistema `exit()` termina un processo in maniera pulita.
1) Mette il processo in stato `EXIT_ZOMBIE.`
2) Attende la lettura del codice di uscita da parte del processo padre.
3) Libera le risorse utilizzate.
4) Mette il processo in stato `EXIT_DEAD.`
5) Distrugge il PCB.
6) **Schedula** l'esecuzione di un altro processo.

**Processi Orfani e Child Reaping**
Se un processo genitore termina i suoi figli diventano **orfani**.
- **Reparenting**: Il meccanismo dove gli **orfani** diventano figli del processo `init.`

**Organizzazione per Gruppi**
Ogni comando inputtato nella **shell** crea un **gruppo** di processi individuato da un **ID di gruppo** detto **PGID** pari all'ID del primo processo in **pipeline** detto **process group leader**.
- In questo modo è possibile inviare un segnale al leader per uccidere tutti i processi coinvolti in un comando. Per il comando `ls -al | grep .bash | less -Mr` abbiamo:

![](Organizzazione-Gruppi.png)

**Organizzazione per Sessioni**
Una **sessione** è un insieme di gruppi di processi che condividono un **terminale di controllo** come il terminale di login. È individuata tramite **ID di sessione** (**SID**). L'ID di sessione è quello del **session leader**, il processo che crea la sessione con la chiamata di sistema `setsid().`

**Gruppo in Foreground e Background**
In ogni istante solo un gruppo di processi può leggere dal terminale. Il gruppo prende nome di **foreground process group**. Gli altri gruppi di processi che non godono di questa proprietà prendono il nome di **background process group**.

``` BASH
echo $$
	400 # PID di BASH

# Pipeline eseguita in background. Abbiamo due processi legati in pipe. Il
# comando non può leggere input da terminale.

find / 2> /dev/null | wc -l & 
	[1] 659 # PID dell'ULTIMO processo. Il process group leader è PID - 1.

# Pipeline eseguita in foreground. Abbiamo due proecssi legati in pipe. Il
# comando può leggere input da terminale.
sort | uniq -c
```

![](PID-PGID-SID.png)