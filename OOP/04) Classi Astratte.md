**Classe Astratta**
Classe che definisce i **metodi** ma non l'**implementazione**. Non servono a creare **istanze** di classe ma a definire dei **comportamenti** comuni alle sottoclassi. Il loro scopo è **fattorizzare** il comportamento delle varie sottoclassi.

``` Java
public abstract class Veicolo
{
	private String nome;
	private String data;
	private int cilindrata;
	
	public Veicolo(String s) {nome = s;}
	
	public abstract String chi_sei();
	public abstract String si_muove();   // Ambiente
	public abstract String trasporta();  // Cosa trasporta
	
	public String toString();
	{
		return nome + ", " + chi_sei() + ", si muove " + si_muove() +
		" e trasporta " + trasporta() + ".";
	}
}
```

**Note**
- Una classe che ha anche **solo UN** metodo astratto è una classe astratta.
- Una classe astratta può **NON** contenere metodi astratti. Non è istanziabile.
- Se una **sottoclasse** non **ridefinisce** i metodi astratti anch'essa è una classe astratta.

**Costruttori in Classi Astratte**
Anche se non possiamo **istanziare** classi astratte tornano utili per **inizializzare** lo stato della classe astratta attraverso una **chiamata del costruttore** da parte delle sottoclassi.

**Criteri di Classificazione**
Dato che **Veicolo** è una classe astratta bisogna creare delle **sottoclassi concrete**. Per definire una **gerarchia** bisogna scegliere il criterio di classificazione (i metodi **astratti**).
- In questo caso possiamo scegliere tra **l'ambiente** o il **trasporto**.

**Ambiente**
Partendo dall'ambiente abbiamo le classi **Veicolo Terrestre** e **Veicolo Acquatico**. Esse saranno a loro volta classi astratte dato che non definiamo ciò che trasportano.

``` Java
public abstract class VeicoloTerrestre extends Veicolo
{
	public VeicoloTerrestre(String s) {super(s);}
	
	@Override
	public String chi_sei() {return "un veicolo terrestre";}
	
	@Override
	public String si_muove() {return "sulla terraferma";}
}

public abstract class VeicoloAcquatico extends Veicolo
{
	public VeicoloAcquatico(String s) {super(s);}
	
	@Override
	public String chi_sei() {return "un veicolo acquatico";}
	
	@Override
	public String si_muove() {return "nell'acqua";}
}
```

**Trasporto**
La gerarchia si espande con cosa **trasportano** i veicoli. I veicoli terrestri si suddividono in **Automobili** che trasportano persone e **Camion** che trasportano merci.
- Classi **concrete**, quindi istanziabili.

``` Java
public class Automobile extends VeicoloTerrestre
{
	public Automobile(String s) {super(s);}
	
	@Override
	public String chi_sei() {return "una auto";}
	
	@Override
	public String trasporta() {return "persone";}
}

public class Camion extends VeicoloTerrestre
{
	public Camion(String s) {super(s);}
	
	@Override
	public String chi_sei() {return "un camion";}
	
	@Override
	public String trasporta() {return "merci";}
}

_______________________________________________________________________________

public class Main
{
	public static void main(String args[])
	{
		Automobile A = new Automobile("Volvo AA111BB");
		Camion C = new Camion("Fiat ZZ999XX");
		
		VeicoloTerrestre v = new VeicoloTerrestre() // ERRORE! Classe Astratta
		
		System.out.println(A.toString());
		System.out.println(C.toString());
	}
}

$ java Main

// Volvo AA111BB, una auto, si muove sulla terraferma e trasporta persone.
// Fiat ZZ999XX, un camion, si muove sulla terraferma e trasporta merci.
```

**Array di Classe Astratta**
L'operazione è legale perchè non stiamo istanziando alcun oggetto.

``` Java
public class Città
{
	public static void main(String args[])
	{
		Veicolo v[] = new Veicolo[4];
		v[0] = new Camion("Fiat ZZ999XX");
		v[1] = new Automobile("Volvo AA111BB");
		v[2] = new Camion("Mercedes MM123NN");
		v[3] = new Automobile("Volkswagen GG777CC");
		
		for (int i = 0; i < 4; i++)
			System.out.println(v[i].toString()); // Stampa veicoli
	}
}
```

**Ereditarietà e Classificazione**
Per la classificazione abbiamo a disposizione **due criteri**, è indifferente da dove si inizia.
- Partendo dall'ambiente perdiamo il concetto di **trasporto**, partendo dal trasporto perdiamo l'**ambiente**. I concetti si dividono tra le classi.

**Ereditarietà Multipla**
È molto **potente**, risolve il problema citato sopra però è **difficile** da gestire. Non presente in Java, infatti si usano le **interfacce**.

**Interfaccia**
Insieme di **dichiarazioni** di metodi, quindi senza implementazione.
- Una classe implementa un'interfaccia se ne **definisce** ogni suo metodo.
- **Ereditarietà multipla** per le interfacce. Una classe può implementarne zero o più.

``` Java
public interface VeicoloPersone
{
	public void viaggia();
	...
}

public class Automobile extends VeicoloTerrestre implements VeicoloPersone
{
	@Override
	public void viaggia()
	{
		// Definizione metodo
	}
}

public interface AutoDiplomatica extends VeicoloPersone, AccessoDiplomatico
{
	... // Ereditarietà multipla
}
```

**Interfacce e Metodologia**
L'aspetto **progettuale** è solitamente implementato con le interfacce mentre l'aspetto **implementativo** con le classi.