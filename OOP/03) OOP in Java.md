**Incapsulamento**
Una classe può contenere **attributi** di qualsiasi tipo, **metodi** e altre **classi**. L'oggetto **incapsula** stato ed interfaccia di una classe.

**Visibilità**
Le variabili ed i metodi possono essere di tipo:
- **Private**: Non visibile dall'esterno (attributi).
- **Public**: Visibile a tutti (metodi).
- **Protected**: Visbile solo dalle **classi figlie** e dalle classi dello **stesso package**.

**Esempio Incapsulamento**

``` Java
public class Veicolo
{
	private int velocitàMassima;
	private int numeroPosti;
	
	public Veicolo(int VM, int NP)
	{
		velocitàMassima = VM;
		numeroPosti = NP;
	}
	
	public int getVelocitàMassima() {return velocitàMassima;}
	public int getNumeroPosti() {return numeroPosti;}
}

public class Esempio2
{
	public static void main(String args[])
	{
		Veicolo miaMacchina; // Incapsulamento da parte dell'oggetto
		miaMacchina = new Veicolo(150, 5);
		
		System.out.print("La mia macchina ha ");
		System.out.print(miaMacchina.getNumeroPosti() + " posti");
		System.out.print(" e raggiunge la velocità di ");
		System.out.print(miaMacchina.getVelocitàMassima() + "km/h.");
		
		// int intero = miaMacchina.numeroPosti - Errore! Inaccessibile
	}	
}

$ java Esempio2
// La mia macchina ha 5 posti e raggiunge la velocità di 150km/h.
```

**Ereditarietà**
Caratteristica della OOP che definisce **nuove classi** partendo da classi esistenti. La nuova classe **eredita** tutti gli attributi e metodi della classe originale. Ne può aggiungere altri e può **ridefinire** i metodi ereditati.
- Si implementano **nuove funzionalità** a classi già esistenti.

``` Java
class Contatore2 extends Contatore
{
	public void dec() {setVal(getVal()-1);}
	// Se val fosse accessibile potevamo scrivere val--
}
```

**Ereditarietà in Java**
- **Ereditarietà Singola**: La classe figlia eredita tutti gli attributi ed i metodi ma può gestire solo quelli **visibili** che non vengono ridefiniti.
- **Ereditarietà Monotona**: Si possono aggiungere solo membri, non rimuoverli.

**Costruttori**
**Non** vengono ereditati dalla classe figlio. Se non viene inserito ne viene aggiunto uno dal **compilatore** (senza parametri) che **richiama** il costruttore senza parametri della superclasse.
- In questo caso se il costruttore senza parametri non esiste nella superclasse si verifica un errore.

**Super**
La keyword `super` ha diverse funzionalità:
- Specificare che si sta facendo riferimento ad un attributo/metodo della classe padre.
- Chiamare un costruttore della classe padre.

``` Java
class ContatorePreciso extends Contatore
{
	private double val; // Campo val di ContatorePreciso
	                    // super.val rappresenta il valore della superclasse
	
	@Override           // Ridefinizione metodo
	public void inc() {super.inc(); val++;}	// Metodo sottoclasse
											// super.inc() metodo superclasse
}
```

**Override**
- **Attributi**: Di solito porta confusione, quindi è una bad practice.
- **Metodi**: Fondamentale per avere un comportamento polimorfico. Il metodo della superclasse è chiamabile attraverso `super.`

**Gerarchia Classi Java**
Tutte le classi **ereditano** implicitamente o meno da una **classe radice** che raggruppa tutte le definizioni comuni ad ogni oggetto, la classe `Object.`

**Ereditarietà Esempio**

``` Java
public class Veicolo /* extends Object */
{
	private int numeroPosti;
	
	public Veicolo(int NP) {numeroPosti = NP;}
	
	public int getNumeroPosti() {return numeroPosti;}
}

public class VeicoloTerrestre extends Veicolo
{
	private int numeroRuote;
	
	public VeicoloTerrestre(int NP, int NR)
	{
		super(NP);
		numeroRuote = NR;
	}
	
	public int getNumeroRuote() {return numeroRuote;}
}

public class VeicoloAcquatico extends Veicolo
{
	private long stazza;
	
	public VeicoloAcquatico(int NP, long S)
	{
		super(NP);
		stazza = S;
	}
	
	public long getStazza() {return stazza;}
}

public class Esempio3
{
	public static void main(String args[])
	{
		VeicoloTerrestre miaMacchina;
		VeicoloAcquatico miaNave;
		
		miaMacchina = new VeicoloTerrestre(5, 4);
		System.out.print("La mia macchina ha ");
		System.out.print(miaMacchina.getNumeroPosti() + "posti e ");
		System.out.println(miaMacchina.getNumeroRuote() + "ruote");
		
		miaNave = new VeicoloAcquatico(10, 10);
		System.out.print("La mia nave ha ");
		System.out.print(miaNave.getNumeroPosti() + "posti e ");
		System.out.println("una stazza di " + miaNave.getStazza());
	}
}

$ java Esempio3
// La mia macchina ha 5 posti e 4 ruote
// La mia nave ha 10 posti e una stazza di 10
```

**Compatibilità tra Classi**
Prendendo come esempio la classe **Contatore**
- Un contatore esteso è un **sottotipo** della classe originale. Posso usarlo come contatore senza le funzionalità aggiunte. **Classificazione** delle entità.

``` Java
// Regola di Conformità

public static void main(String args[])
{
	Contatore c1 = new Contatore(3);
	Contatore2 c2 = new Contatore2();
	
	c2.dec(); // Si: Esiste in Contatore2
	c1.dec(); // No: Non esiste in Contatore2
	c1 = c2;  // Si: Contatore2 è anche Contatore
	c2 = c1;  // No: Contatore non è un Contatore2
}
```

**Polimorfismo**
Caratteristica della OOP che permette ad una **classe** di fornire diverse **implementazioni di metodi** inizialmente definiti nella classe base. Si basa su:
- **Binding Dinamico**: Associazione dinamica tra variabile ed oggetto (tipo `V = A1`).
- **Operation Dispatching**: Ricerca dinamica dei metodi in memoria.
- **Overriding**: Sovrascrizione dei metodi in modo da cambiare il comportamento.
- **Regola di Conformità**: Permette l'assegnamento da superclasse a sottoclasse.

**Polimorfismo Esempio**

``` Java
public class Veicolo /* extends Object */
{
	private int numeroPosti;
	
	public Veicolo(int NP) {numeroPosti = NP;}
	
	public int getNumeroPosti() {return numeroPosti;}
	public String toString() 
	{return "Veicolo con " + NumeroPosti + " posti"};
}

public class Topolino extends Veicolo
{
	public Topolino(int NP) {super(NP);}
	
	@Override
	public String toString() 
	{return "Sono una Topolino e ho " + getNumeroPosti() + " posti"};
}

public class SeicentoFamiliare extends Veicolo
{
	public SeicentoFamiliare(int NP) {super(NP);}
	
	@Override
	public String toString()
	{return "Sono una SeicentoFamiliare e ho " + getNumeroPosti() + " posti"};
}

public class Esempio
{
	public static void main(String args[])
	{
		Veicolo V;
		Topolino A1 = new Topolino(4);
		SeicentoFamiliare A2 = new SeicentoFamiliare(6);
		
		V = A1;
		System.out.println(V.toString());
		V = A2;
		System.out.println(V.toString());
	}
}

$ java Esempio
// Sono una Topolino e ho 4 posti
// Sono una SeicentoFamiliare e ho 6 posti
```

![[MemoriaJava.png]]