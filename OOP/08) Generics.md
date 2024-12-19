**Type-Safety**
Un programma type-safe è un programma che a compile time garantisce la **coerenza** dei dati di una struttura dati. I **generics** garantiscono la type-safety.

**Esempio Type-Unsafety**
La classe Archivio1 gestisce con una sola logica tutti i tipi **Persona**, ciò provoca programmi **type-unsafe**. Non ci sono **controlli** che vietano l'aggiunta del tipo sbagliato nell'archivio.
- Le incoerenze possono non essere rilevate a tempo di compilazione!

``` Java
// Il programma rappresenta un archivio di studenti e professori. Deve
// essere omogeneo, non dobbiamo mischiare le persone.

public class Persona // Generalizzazione
{
	private String nome, cognome;
	private int eta;
	
	public Persona(String nome, String cognome, int eta)
	{
		this.nome = nome;
		this.cognome = cognome;
		this.eta = eta;
	}
	
	public String toString()
	{
		return nome + " " + cognome + " " + eta;
	}
}
```

``` Java
public class Professore extends Persona
{
	private String materia;
	
	public Professore(String nome, String cognome, int eta, String materia)
	{
		super(nome, cognome, eta);
		this.materia = materia;
	}
	
	public String getMateria()
	{
		return materia;
	}
}
```

``` Java
public class Studente extends Persona
{
	private int matricola;
	
	public Studente(String nome, String cognome, int eta, int matricola)
	{
		super(nome, cognome, eta);
		this.matricola = matricola;
	}
	
	public String getMatricola()
	{
		return matricola;
	}
}
```

``` Java
import java.util.Vector;

public class Archivio1
{
	private Vector persone;
	
	public Archivio1()
	{
		persone = new Vector(10);
	}
	
	public void aggiungi(Persona p)
	{
		persone.add(p);
	}
	
	public void rimuovi(Persona p)
	{
		persone.remove(p);
	}
	
	public Persona get(int index)
	{
		return (Persona)persone.get(index);
	}
	
	public int size()
	{
		return persone.size();
	}
}
```

**Generics**
È un tipo di dato che esegue il controllo **a tempo di compilazione**. Le strutture dati devono essere progettate come tipi **parametrici**. Una classe che sfrutta generics non ha un tipo di dato predefinito ma lo **riceve** a tempo di **istanziazione**.
- Abbiamo più **tipi di dati** utilizzabili in modo **coerente**. La sintassi è `<Tipo>.`
- Non viene impedito l'uso di **gerarchia**, basta essere consci di come usarli.

``` Java
// Esempio precedente corretto

import java.util.Vector;

public class Archivio2<E> // Identificatore E = La classe usa generics
{
	private Vector persone;
	
	public Archivio2()
	{
		persone = new Vector(10);
	}
	
	public void aggiungi(E p)
	{
		persone.add(p);
	}
	
	public void rimuovi(E p)
	{
		persone.remove(p);
	}
	
	public E get(int index)
	{
		return (E)persone.get(index);
	}
	
	public int size()
	{
		return persone.size();
	}
}
```

``` Java
public main
{
	public static void main(String args[])
	{
		Archivio2<Persona> archivio_per = new Archivio2<Persona>();
		Archivio2<Studente> archivio_stud = new Archivio2<Studente>();
		Archivio2<Professore> archivio_prof = new Archivio2<Professore>();
		
		archivio_prof.aggiungi(s1); // ERRORE! Generic fa il controllo tipo
		archivio_prof.aggiungi(pr1);
		archivio_prof.aggiungi(pr2);
	}
}
```

**Wildcards**
Le wildcards dei generics si indicano col carattere `?` e hanno diversi scopi:
- `<? extends type>:`  Indica tutti i tipi che ereditano da `type.` Quindi `Archivio2<? extends Persona>` indica tutti i tipi di `Archivio2` parametrizzati da `Persona.` Non è quindi possibile usare classi diverse rispetto a quelle di `Persona.`
- `<? super type>:` Simile al caso precedente, tratta superclassi.

``` Java
// Rende possibile assegnamento da variabile a oggetto parametrizzato

Vector<? extends Persona> v = new Vector<Studente> // OK
Vector<Persona> v = new Vector<Studente>           // Errore senza wildcard
```

**Specializzazione Classi Generics**
È possibile specializzare una classe ed aggiungere il supporto a generics, facendo attenzione al fatto che i metodi non siano in conflitto.