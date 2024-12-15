**Sviluppo Software**
Un software si può scrivere con un qualsiasi linguaggio che abbia potenza computazionale del while. I paradigmi di programmazione standard rispettano ciò ma non è ottimale creare dei software con la programmazione **modulare** o **funzionale**.
- Tempi di scrittura lunghi, protezione dei dati complessa, operazioni illegali ammesse...

**Soluzione**
Un linguaggio di programmazione che fornisca **nativamente** dei costrutti che garantiscono un alto livello di **astrazione**, una maggiore **protezione** di dati ed un facile **riutilizzo** di codice attraverso **entità** che uniscono attributi ed operazioni.

**Esempio**

``` C
// Contatore
ADTDef Contatore
{
	// Attributi non accessibili
	int val;
	
	// Operazioni ammesse
	void setVal(int newVal) {val = newVal;}
	void inc()              {val++;}
	int getVal()            {return val;}
}

// Utilizzo
int main()
{
	Contatore cont1;
	Contatore cont2; // Posso istanziare due oggetti Contatori
	
	cont1.setVal(0);
	cont2.setVal(1);
	cont1.inc();     // Incremento il primo contatore
	cont2.inc();     // Incremento il secondo contatore senza toccare il primo
	
	cont++;          // Errore! Il contatore non è un intero.
	cont.val++;      // Errore! Accedo a val solo tramite le operazioni.
}
```

**Protezione dei Dati**
Il controllo avviene nelle operazioni, quindi incluso nelle entità in modo da codificarlo una volta sola. Se viene messo un valore che non rispetta il controllo il compilatore da errore.

**Oggetto**
Entità che incapsula attributi ed operazioni e li unisce. È composto da uno **stato** e da una **interfaccia** ed è un astrazione di dato.

**Stato**
Costituito dagli **attributi** dell'oggetto. **Nascosto all'esterno**. Solo le operazioni della stessa classe possono interagire con lo stato.

**Interfaccia**
Costituita dalle **operazioni** che possono essere invocate dall'esterno.

**Classi**
Descrive la **struttura comune** a più oggetti in termini di attributi ed operazioni. È un **tipo di dato astratto**, funziona come una sorta di **stampino** per creare oggetti.

**Classi e Oggetti**
Le classi sono definite **staticamente** dal programmatore, gli oggetti sono **dinamicamente** creati dal programma quando servono.

**Modello Client-Server**
- **Client**: Richiede il servizio e conosce direttamente il server, inoltre ne è indipendente.
- **Server**: Mette a disposizione il servizio, non conosce il server a priori (**oggetto**).

``` C
// Procedurale
inserisci(Tipo_Dato, Elemento) // Funzione che agisce su entità

// Ad Oggetti
Tipo_Dato.inserisci(Elemento)  // Entità che svolge un servizio
```

**Filosofia Object-Oriented**
- Un applicazione è un insieme di oggetti che interagiscono.
- L'interazione tra oggetti avviene tramite metodi.
- Rapporto client-server tra oggetti.

**Classe**
- **Classificazione**: Insieme di oggetti che possono condividere lo stesso comportamento.
- **Classe**: Entità che raggruppa tutti gli attributi e le operazioni che un oggetto avrà.
- **Oggetto**: Un'istanza di una classe.

**Oggetto di una Classe**
Forte dipendenza tra la classe e un oggetto, perdura fino al tempo di vita dell'oggetto.