**Eccezione**
Un **eccezione** è un evento che capita durante l'esecuzione di un programma e ne sconvolge il suo comportamento. La **gestione degli errori** in Java viene svolta attraverso le eccezioni.

**Gestione Tradizionale di Errori**
Principalmente si svolge usando il costrutto **if-then**. Nell'OOP può causare problemi:
- Un costrutto if-then per **ogni** funzione rende il programma complesso.
- Non possiamo restituire un qualsiasi numero intero.

**Gestione Eccezioni in Java**
Viene usato il costrutto **try-catch**. Esistono **classi** che rappresentano le eccezioni. La **radice** della gerarchia delle eccezioni è la classe **Throwable** che ha due sottoclassi:
- **Error**: Definisce errori NON recuperabili.
- **Exception**: Definisce **anomalie** che possono essere recuperate.

**Vantaggi**
- Definizione delle **entità** che rappresentano le eccezioni.
- Non c'è confusione tra **valore di ritorno** e **segnalazione di un'eccezione**.
- Le eccezioni sono **oggetti**, contengono **variabili/metodi** per far capire cosa è successo.

**Sollevare un'Eccezione**
- **Throws**: Indica quali eccezioni possono essere sollevate.
- **Throw**: Solleva un eccezione.

**Catturare un'Eccezione**
Si usa il **try-catch** per gestire un'eccezione sollevata. Ci possono essere più catch per catturare più eccezioni.
- Per esempio il metodo `read()` può sollevare una `IOException` se lettura da file fallisce.
- Non tutte le eccezioni vanno catturate, esempio le `RuntimeException.`

``` Java
try
{
	// Il metodo a rischio va invocato qui dentro
	c = f.read();
}
catch (IOException e) // <tipo_eccezione> <nome_variabile>
{
	// Istruzioni da eseguire in caso di eccezione
	System.err.println("Impossibile leggere: " + e);
}
```

**Rilanciare un'Eccezione**
Un metodo che invoca un altro metodo **a rischio** può decidere di **rilanciare** l'eccezione al chiamante invece di gestirla.
- La chiamata di `m()` dev'essere all'interno di un try-catch.

``` Java
public int m() throws IOException
{
	c = f.read();
}
```

**Package I/O**
Il package definisce quattro classi base **astratte**:
- **Input/Output Stream**: Lettura/scrittura di **byte** (`!= char`).
- **Reader/Writer**: Lettura/scrittura di **caratteri**.

**Byte vs Caratteri**
Sono due tipi **diversi** in Java. I caratteri pesano **due** byte invece che uno, quindi è necessario avere due tipi di classi diverse per l'I/O.

**Classi Derivate**
Le classi astratte danno origine ad una varietà di classi **concrete** che leggono/scrivono tipi specifici di stream. Abbiamo quindi delle **sottoclassi** che si dividono in **sorgente** e **filtro**.

**Classi Sorgente/Pozzo**
Non aggiungono funzionalità, bensì specificano la **sorgente/destinazione** dei flussi.
- Sia per l'input che per l'output il flusso può diventare un **file** o un **buffer**.

**Classi Filtro**
Ampliano le funzionalità delle classi astratte. Oltre a byte/caratteri fanno in modo che si possano scrivere/leggere da file anche tipi primitivi, oggetti etc...

**Incapsulamento (non OOP)**
L'I/O si usa componendo le **classi concrete** che ci interessano in modo da avere una **serie** di oggetti che svolgono ogni funzionalità richiesta.
1) Si crea un oggetto da una classe **sorgente** per definire il flusso di dati.
2) Si crea un oggetto da una classe **filtro** per definire ciò che dobbiamo leggere/scrivere, si passa al costruttore l'oggetto **sorgente** come per incapsularlo.

``` Java
import java.io.*;

public class IO
{
	public static void main(String args[])
	{
		FileInputStream f = null;
		f = new FileInputStream("Prova.dat");
		
		// Stream di input associato ad un file, classe sorgente.
		
		DataInputStream is = new DataInputStream(f);
		
		// Viene incapsulato in una classe filtro che da le funzionalità
		// necessarie per leggere i tipi primitivi.
	}
}
```

**I/O Binario - Apertura Stream**
Lo stream viene aperto il momento in cui l'oggetto viene **istanziato**.

**Lettura Stream**
Si usa il metodo `read()` che restituisce un int (4 byte) che vale `0-255` se la lettura è andata a buon fine e `-1` se lo stream è finito.

``` Java
int i; byte b;
i = in.read();

if (i != -1)      // Si usa l'intero per controllare lo stato dello stream
	b = (byte) i; // Cast a byte per poter utilizzare un byte specifico
```

**Check Fine Stream**
Si usa il metodo `read().` Se il valore è -1 allora il file è finito.

**Serializzazione**
La serializzazione è la trasformazione di un oggetto in uno **stream di byte** salvabile su file.
- Una classe serializzabile deve implementare l'interfaccia `Serializable.`

