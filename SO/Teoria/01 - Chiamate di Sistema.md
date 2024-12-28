**SO e Programmi Applicativi**
- **Sistema Operativo**: Composto dal **kernel** che fa da mediatore tra HW e applicazioni e dal **sistema di base** che permette all'SO di avviarsi e mostrare un'**interfaccia di testo**.
- **Programmi Applicativi**: Tutto ciò che non è kernel/sistema di base.

![](SO.png)

**Interazione Applicazione-Kernel**
Un'applicazione (di base o meno) invia una **richiesta** al kernel. Il kernel quindi invia una **risposta** all'applicazione. Viene usato il modello **client-server**.

![](App-Kernel.png)

**User Mode e Kernel Mode**
- **User Mode**: Un'app in user mode ha **privilegi ridotti**. Non può alterare memoria di altre applicazioni o del kernel (esempio). **Protezione** contro usi maliziosi/sbadati.
- **Kernel Mode**: Si ha quando un'app richiede un servizio. Essa avrà **privilegi massimi**, è lei che si occupa di eseguire il codice del servizio richiesto al kernel. Una volta che finisce il servizio, l'app ritorna in user mode.

**User Space e Kernel Space**
- **User Space**: Indirizzi di memoria accessibili ad un'applicazione.
- **Kernel Space**: Indirizzi di memoria accessibili al kernel.

**Livelli di Privilegio CPU Intel**
Nelle CPU Intel sono previsti quattro **anelli** di privilegio. In GNU/Linux ne vengono usati due; il **livello zero** rappresenta la kernel mode ed il **livello tre** rappresenta la user mode.

**Chiamata di Sistema**
L'unico meccanismo con cui un'applicazione riesce a chiedere un servizio al kernel.

**Passaggio dei Parametri**
- Una **syscall** accetta al più sei parametri. Per averne di più serve come parametro un **puntatore ad una struttura dati**. I parametri vengono passati tramite registri.
- Il registro **eax** contiene sempre un ID numerico della syscall. Essi vengono contenuti in `/usr/include/asm-generic/unistd.h`

**Ingresso in Kernel Mode**
- **<Pentium 2**: Si usava la **trap 128** ovvero l'istruzione `int $0x80.` `int` sta per interruzione (software) e il numero `$0x80 (128 in dec)` è il numero di interruzione, invoca le syscalls.
- **>Pentium 2**: Si usa l'istruzione assembly `sysenter.`

**Invocazione Funzione di Servizio**
`system_call()` è una funzione di **servizio del kernel** ed è la prima funzione eseguita in kernel mode. Il suo compito è invocare la funzione di servizio richiesta dall'applicazione.

**Funzione di Servizio**
Le **funzioni del kernel** hanno il prefisso `sys_.` Se si invoca la chiamata di sistema `getpid()` allora nel kernel esisterà sicuramente una funzione `sys_getpid().`

**Ritorno in User Space**
Il valore di ritorno della funzione di servizio è salvato nel registro **eax**. Se per entrare in kernel mode si è usata l'istruzione `sysenter` per uscire si usa la `sysexit.`

**Libreria C**
Invocare i servizi del kernel in questo modo è molto complicato. Linux usa la **GNU C Library**, una libreria wrapper, per facilitarne l'uso.

![](Wrapper-C.png)

**Convenzioni di Chiamata**
Convenzioni standard del C.
- I parametri vengono spinti sullo **stack in ordine inverso**, segue una jump to subroutine (salto alla funzione) tramite istruzione `call.`