**Caratteristiche**
È un linguaggio imperativo, fortemenete tipizzato, **ad oggetti, interpretato** e portabile. L'allocazione della memoria si fa attraverso la keyword `new`, la deallocazione è gestita dal **garbage collector**. L'ambiente di sviluppo usato è il **JDK**.

**Classi ed Istanze**
- **Tipi Primitivi**: Byte, short, int, long, float, double, char...
- **Tipi Riferimento**: Oggetto istanza referenziato da una variabile.

**Assegnamento**
Viene svolto tramite l'operatore `=` seguendo la logica `l-value = r-value`.
- L-value è una variabile, r-value un espressione. `a = 5`.

**Cast**
Si può forzare il cambio del tipo di una variabile tramite il cast.

``` Java
int i;
float f = 5.15F;
i = (int)f; // Perdita di informazioni senza questo cast
```

**Operatori Aritmetici, Booleani, Bitwise e Confronto**
- Somma, sottrazione, moltiplicazione, divisione, resto, incremento, decremento...
- Or, and, not logico. Funzionano in **cortocircuito**.
- Or, and bitwise, shift a sinistra/destra di **N** posizioni.
- Uguale a, diverso da, maggiore, maggiore/uguale, minore, minore/uguale.

**If**
Esegue uno statement se la condizione è vera.

``` Java
if (a > 10) // Condizione
{
	b = 1;
}
else        // Altrimenti
{
	b = -1;
}
```

**For**
Esegue un assegnamento iniziale, ripete un blocco di istruzioni finchè la condizione è vera, esegue un incremento a fine iterazione.

``` Java
for (int i = 0; i<10; i++)
{
	a = a * 2;
}
```

**While**
Esegue un blocco di istruzioni finchè la condizione è vera.

``` Java
while (i < 10)
{
	i = i + 1;
}
```

**Do-While**
Simile al while. Il controllo viene fatto dopo l'esecuzione delle istruzioni quindi verrà eseguito minimo una volta.

``` Java
do
{
	i = i + 1;
}   
while (i < 10);
```

**Switch**
Esegue il blocco di istruzioni corrispondente al valore della variabile.

``` Java
String day = "Monday";

int dayOfWeek = switch(day)
{
	case "Monday" -> 1;
		break; // Senza verrebbero eseguite tutte le istruzioni fino in fondo
	case "Tuesday" -> 2;
		break;
	...
	default -> -1;
} ;
```

**Funzioni**
Detti metodi in Java, possono essere definiti **solo** dentro una classe. Possono esistere metodi con **stesso nome** ma numero/tipo di parametri **diverso**.

``` Java
int somma(int a, int b)
{
	return a + b;
}
```

**Commenti**

``` Java
1) Commenti C   /* Ciao */
2) Commenti C++ // Ciao
3) Commenti Doc /** Ciao */
```

**Classe**
Insieme di **stato** ed **interfaccia**. Template che definisce metodi e operazioni per un oggetto. Usato per creare nuove variabili, creiamo oggetti con la keyword `new.`
- **Classe Wrapper**: Classe che aggiunge funzionalità ai **tipi di dato** primitivi in modo da usarli come oggetti (Byte, Short, Integer...)

**Costruttore**
Metodo speciale che ha il compito di **inizializzare** l'oggetto. Ha lo stesso nome della classe, non ha un valore di ritorno, **può essere più di uno** se si cambiano i parametri.
- Se non viene scritto dal programmatore viene aggiunto un costruttore senza parametri con corpo vuoto.

``` Java
class Contatore
{
	private int val;              // Stato
	
	public Contatore() {val = 0;} // Costruttore
	
	public void setVal(int newVal) {val = newVal;} //
	public void inc() {val++;}                     // Metodi
	public int  getVal() {return val;}             // 
}
```

**Creazione dell'Oggetto**

``` Java
Contatore cont;
cont = new Contatore();

// cont è una variabile che contiene un riferimento ad un'area di memoria dove
// è contenuto il nuovo oggetto creato.
```

**Metodo Main**
L'esecuzione del programma Java inizia dall'esecuzione di un metodo speciale che può creare i oggetti ed invocare i metodi.
- `public static void main(String args[])`

**Esecuzione del Programma**
- **Compilazione**: Crea il bytecode del programma che è il formato intermedio da passare all'interprete tramite `javac nomefile.java.`
- **Interpretazione**: Esegue il programma. Bisogna passargli il nome della classe che contiene il metodo main, tipo `java nomeclasse.`

**Primo Programma**

``` Java
public class PrimoEsempio
{
	public static void main(String args[])
	{
		Contatore c;
		c = new Contatore(); // Dichiarazione + allocazione in memoria
		
		c.setVal(0);
		c.inc();             // Eseguite operazioni
		
		System.out.print("Valore: ");
		System.out.println(c.getVal());
		System.out.println("Ciao Mondo!");
		
		// Stampa del contatore + Ciao Mondo!
	}
}

$ javac PrimoEsempio.java // Compilazione
$ java PrimoEsempio       // Interpretazione

// Output
// Valore: 1
// Ciao Mondo!
```

**Stringhe**
In Java sono **oggetti** e non sono buffer modificabili (bisogna usare **Stringbuffer**).
Esistono molti metodi riguardanti le stringhe, basta consultare la doc. di Java.

``` Java
String s = "Ciao";                // Creazione oggetto, valore "Ciao"
String s = "Ciao" + " " + "Pippo" // Concatenazione di stringhe
char ch = s.charAt(2);            // Selezione di un carattere (vale "i")
```

**Arrays**
In Java sono **oggetti** e vengono identificati da brackets. Definiamo un riferimento. L'array va creato con `new` a meno che non abbia già dei valori preassegnati.
- La lunghezza dell'array è data da `length.`
- Creare un array con `new` vuol dire creare **N** celle di un tipo primitivo se l'array è di tipi primitivi oppure **N** riferimenti ad oggetti **NULL** se l'array è di oggetti.

``` Java
class EsempioArray
{
	public static void main(String args[])
	{
		int[] v1 = {8, 12, 3, 4}; // Vettore di 4 int
		int[] v2;                 // Riferimento a vettore
		v2 = new int[5]           // Creazione vettore di 5 int
		
		for (int i = 0; i < v2.length; i++)
			v2[i] = i * i;
			
		for (int i = 0; i < v1.length; i++)
			System.out.print(v1[i] + '\t');
		System.out.println();
		
		for (int i = 0; i < v2.length; i++)
			System.out.print(v2[i] + '\t');
		System.out.println();
	}
}
```

**Documentazione**
Tramite **Javadoc** è possibile generare pagine HTML di documentazione.
- Il comando `javadoc -author -version -d -doc NomeFile.java` crea i file HTML.

**Package**
Sono contenitori **logici** di classi. Corrispondono a contenitori **fisici** dei file .class e possono essere ricorsivi, quindi un package può contenere sottopackages.
- Nota: * non è ricorsivo.

``` Java
utilità.Contatore;        // Classe contatore all'interno di utilità
import utilità.Contatore; // Importa la classe contatore del package utilità
import utilità.*;         // Importa tutte le classi del package utilità

// Senza gli import dovremmo usare il nome completo package.classe, scomodo.

package utilità;           // Ad inizio file, inserisce la classe nel package
package utilità.Contatore; // Per sottopackage

public class Contatore
{
	...
}
```

**Variabili e Metodi di Classe**
Definite tramite modificatore `static`, quindi esistono **indipendentemente** dalla creazione di un oggetto. I metodi di classe possono essere invocati direttamente ed accedono **solo** alle variabili di classe.

**Costanti**
Definite tramite keyword `final.`  Il valore della variabile non può cambiare.

**Riferimenti ad Oggetti**
I riferimenti contengono un riferimento ad un oggetto.
- `p2 = p1` non duplica `p1` ma **copia il riferimento all'oggetto contenuto** in `p1` quindi entrambi punteranno allo stesso oggetto.
- Per clonare un oggetto usiamo il metodo `clone().`
- Per confrontare il contenuto di due oggetti usiamo il metodo `equals().` Non possiamo usare l'operatore di uguaglianza perchè **confronta i riferimenti**, quindi restituisce true se entrambi i riferimenti puntano allo stesso oggetto.

**Passaggio dei Parametri**
Vengono passati per **valore**. Se l'oggetto è di tipo riferimento viene copiato il riferimento. Se la funzione modifica il riferimento esse **non verranno salvate**.

**Garbage Collector**
Componente dell'interprete che si occupa di **liberare automaticamente la memoria** una volta che non è più accessibile.

``` Java
Contatore c = new Contatore();
c = null; // Garbage Collector può liberare questa memoria
```

**This**
Ha due funzioni:
- Risolvere **ambiguità**, serve per chiamare **variabili** e **metodi** di istanza.
- Chiamare costruttori **della stessa classe**.

``` Java
class Contatore
{
	private int val;
	
	public Contatore(int val) {this.val = val;} // Si riferisce al 1o attributo
	public Contatore() {this(0);}               // Costruttore senza parametri
}
```