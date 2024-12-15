**Classi Wrapper**
Classi create per supportare la gestione dei **tipi primitivi** come gli altri oggetti.
- Risultano **scomode** il più delle volte.

``` Java
Vector v = new Vector();
int i = 10;

Integer iObject = new Integer(i);
v.add(iObject);

iObject = (Integer)v.get(0);
i = iObject.intValue();
```

**Autoboxing - Outboxing**
Consiste nell'usare direttamente uno **scalare** in contesto di oggetto. Il compilatore si occupa automaticamente di gestire il **wrapping** del tipo primitivo.

``` Java
Vector v = new Vector(10);

for (int i = 0; i < 5; i++)
{
	v.add(i);
	v.add(((float)i) * 2.5);
}

for (int i = 0; i < v.size(); i++)
	System.out.println(i + "=" + v.elementAt(i));
```

**Foreach**
Consente di scorrere tutti gli elementi di una **struttura dati** gestendo automaticamente l' **incremento** dei vari indici e l'**assegnamento** della variabile del ciclo all'elem. successivo.
- Non è **null-safe**! Sarebbe meglio implementare un controllo sull'array nullo.
- Può essere usato solo come ciclo di **estrazione**, non assegnamento.
- Supporta i generics.

``` Java
// for (tipo_variabile nome_variabile : list)

public class foreach
{
	public static void main(String args[])
	{
		String array[] = new String[10];
		
		for (int i = 0; i < array.length; i++)
			array[i] = new String("Stringa n." + i);
	}
	
	for(String s: array)
		System.out.println(s);
}

// Attenzione al tipo dell'oggetto iterato. I vector per esempio restituiscono 
// Object! Quindi in quel caso il foreach è for (Object var: Vector)
```

**Varargs**
È possibile definire metodi che accettino un numero **variabile** di argomenti. Devono avere tutti lo **stesso tipo**.

``` Java
public void mvar(String...pars)
{
	for (int i = 0; i < pars.length; i++)
		System.out.println("Parametro " + i + " = " + pars[i]);
}

// Equivalente col foreach

/*
public void mvar(String...pars)
{
	for (String s: pars)
		System.out.println(s);
}
*/

public static void main(String args[])
{
	varargs v = new varargs();
	v.mvar("ALFA", "BETA", "GAMMA");
}
```

**Overload Varargs**
È possibile sovraccaricare un metodo varargs con una lista di argomenti dello stesso tipo. Verrà **prima** eseguito il metodo il cui prototipo corrisponde perfettamente all'invocazione.

``` Java
public class varargs
{
	public void mvar(String...pars)
		for (int i = 0; i < pars.length; i++)
			System.out.println("Parametro " + i + " = " + pars[i]);
	
	public void mvar(Object..pars)
		for (Object o: pars)
			System.out.println("Parametro di tipo: " + o.getClass());
	
	public void mvar(String s1, String s2)
		System.out.println("Stringhe " + s1 + " " + s2);
}

// v.mvar("ALFA", "BETA", "GAMMA");       esegue il primo metodo
// v.mvar("Stringa1", "Stringa2");        esegue il terzo metodo
// v.mvar(new Object(), new Integer(10)); esegue il secondo metodo
```

**Enumerazioni**
La keyword `enum` definisce le enumerazioni.

``` Java
public class EventType
{
	public static enum type
	{
		OPEN,
		CLOSE,
		EXIT
	} ;
}
```

