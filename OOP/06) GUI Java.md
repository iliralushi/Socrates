**Java Swing**
Libreria successiva ad **AWT**. Viene usata per implementare **interfacce grafiche**. Ogni componente è un **oggetto** e ne possiamo specializzare le loro classi.
- È quasi interamente indipendente dall'SO, quindi i suoi componenti sono generali.
- I package da importare per l'uso della libreria sono `java.awt` e `javax.swing.`

**Programmazione ad Eventi**
L'interazione con gli utenti viene programmata tramite **eventi** che sono oggetti:
1) Quando un utente compie un azione il sistema **genera** un evento appropriato.
2) L'evento viene inviato ad un **ascoltatore**.
3) L'ascoltatore gestisce l'evento ed esegue codice appropriato.

**JFrame**
È la classe che implementa la **finestra** dell'applicazione.
- Ha un **bordo**, una **barra** e dei **pulsanti** per azioni sulla finestra.

**Metodi JFrame**
- `void setBounds(int x, int y, int width, int height):` Setta le dimensioni della finestra. `(x, y)` determinano la posizione, `(width, height)` la dimensione effettiva.`
- `void setDefaultCloseOperation(int operation):` Specifica il comportamento del frame quando si preme il pulsante X. Di default è settato ad `HIDE_ON_CLOSE.`
- `void setVisible(bool flag):` Determina la visibilità del frame.

``` Java
import java.awt.*;
import javax.swing.*;

public class EsSwing1
{
	public static void main(String args[])
	{
		JFrame f = new JFrame("Esempio 1");
		
		f.setBounds(200, 100, 300, 150);
		f.setDefaultCloseOperation(EXIT_ON_CLOSE);
		f.setVisible(true);
	}
}
```

**Estensione JFrame**

``` Java
// La classe MyFrame estesa comprende due costruttori
// 1) Un costruttore senza parametri
// 2) Un costruttore che inizializza la finestra

import java.awt.*;
import javax.swing.*;

public class MyFrame extends JFrame
{
	public MyFrame() 
	{
		this("");      // Chiama il secondo costruttore di MyFrame
	}
	
	public MyFrame(String titolo)
	{
		super(titolo); // Costruttore JFrame, inizializza il frame
		
		setBounds(200, 100, 300, 150);
		setDefaultCloseOperation(EXIT_ON_CLOSE);
	}
}

public class EsSwing2
{
	public static void main(String args[])
	{
		MyFrame f = new JFrame("Esempio 2");
		f.setVisible(true);
	}
}
```

**Inserzione Componenti JFrame**
Il metodo `add()` permette di aggiungere componenti direttamente al **JFrame**. Solitamente la componente che si aggiunge è un **JPanel**.

``` Java
import java.awt.*;
import javax.swing.*;

public class EsSwing3
{
	public static void main(String args[])
	{
		MyFrame f = new MyFrame("Esempio 3");
		JPanel panel = new JPanel();
		
		f.add(panel);
		f.setVisible(true);
	}
}
```

**JPanel**
È la classe che implementa un **pannello**. Nel panel si inseriscono componenti come etichette, bottoni, disegni. I componenti vanno aggiunti tramite costruttore del pannello.

**JLabel**
È la classe che implementa una **etichetta.**
- A differenza di una stringa disegnata si può cambiare la **scritta**, la **posizione** e può essere ritornato la scritta contenuta.

**JLabel Metodi**
- `pack():` Dimensiona il frame in modo da contenere estattamente il pannello.
- `get/setText():` Recupera il testo contenuto nell'etichetta.

``` Java
import java.awt.*;
import javax.swing.*;

public class Es7Panel extends JPanel
{
	public Es7Panel()
	{
		super();
		JLabel l = new JLabel("Etichetta");
		add(l);
	}
}
```

``` Java
import java.awt.*;
import javax.swing.*;

public class Es7Swing
{
	public static void main(String args[])
	{
		JFrame f = new JFrame("Esempio 7");
		Es7Panel p = new Es7Panel();
		
		f.add(p);
		f.pack();
		
		f.setDefaultCloseOperation(EXIT_ON_CLOSE);
		f.setVisible(true);
	}
}
```

**Ascoltatore**
È un qualsiasi oggetto che **implementa** un interfaccia di tipo **Listener**.

**JButton**
È la classe che implementa il classico **bottone** premibile dall'utente. Genera un **evento** di tipo **ActionEvent** e l'ascoltatore deve implementare l'interfaccia **ActionListener**.

**Ascoltatore JButton**
L'ascoltatore può essere il pannello che contiene l'oggetto che **genera l'evento** oppure un oggetto **esterno**. L'ascoltatore può essere unico oppure ogni bottone ne può avere uno.

**Metodi JButton**
- `void actionPerformed(ActionEvent ev):`  L'ascoltatore deve implementare e scrivere questo metodo. Gestisce l'evento generando codice opportuno.
- `addActionListener():` Registra un oggetto come ascoltatore degli eventi.
- `getActionCommand():` Restituisce la **stringa** associata all'evento.
- `getSource():` Restituisce il riferimento all'oggetto che ha generato l'evento.

``` Java
// L'applicazione contiene un etichetta ed un pulsante. Ogni volta che il
// pulsante viene premuto l'etichetta passa da Tizio a Caio e viceversa.

import java.awt.event.*;
import javax.swing.*;

public class Es8Panel extends JPanel implements ActionListener
{
	private JLabel l;
	
	public Es8Panel()
	{
		super();
		l = new JLabel("Tizio");
		add(l);
		
		JButton b = new JButton("Tizio/Caio");
		b.addActionListener(this);
		this.add(b);
	}
	
	public void actionPerformed(ActionEvent ev)
	{
		if (l.getText().equals("Tizio"))
			l.setText("Caio");
		else
			l.setText("Tizio");
	}
}
```

``` Java
import java.awt.*;
import javax.swing.*;

public class EsSwing8
{
	public static void main(String args[])
	{
		JFrame f = new JFrame("Esempio 8");
		Es8Panel p = new Es8Panel();
		
		f.add(p);
		f.pack(p);
		
		f.setDefaultCloseOperation(EXIT_ON_CLOSE);
		f.setVisible(true);
	}
}
```

**JTextField**
È la classe che implementa una **riga di testo**, usabile per scrivere e visualizzare testo. Genera un **DocumentEvent** (gestito quando vien modificato il testo) ed un **ActionEvent**.
- Il testo è accessibile coi getter/setter.
- Il campo di testo può/non può essere editabile.

**JCheckBox**
È la classe che implementa una **casella di opzione** che può essere de/selezionata. Genera un **ActionEvent** ed un **ItemEvent**.

**Metodi JCheckBox**
- `public void itemStateChanged (ItemEvent e):` L'ascoltatore deve implementare l'interfaccia **ItemListener** e questo metodo. Realizza l'ascoltatore degli eventi.
- `isSelected():` Controlla lo stato di una casella.
- `setSelected():` Modifica lo stato di una casella.
- `getItemSelectable():` Restituisce il riferimento all'oggetto sorgente dell'evento.

**JRadioButton**
È la classe che implementa una **casella di opzione** che fa parte di un gruppo. In ogni istante può essere attiva **una sola** casella del gruppo. L'evento da gestire è l'**ActionEvent**.
1) Si creano i JRadioButton che servono.
2) Si crea un oggetto di tipo **ButtonGroup**.
3) Si aggiungono i JRadioButton al gruppo tramite metodo `add().`

**JList**
È la classe che impementa una **lista di valori**. Si possono scegliere uno o più valori. Genera un evento **ListSelectionEvent** che va gestito al posto dell'**ActionEvent**. L'ascoltatore deve implementare l'interfaccia **ListSelctionListener**.

**Metodi JList**
- `void valueChanged(ListSelectionEvent e):` Gestore dell'evento.
- `getSelectedValue/s():` Restituisce la/le voci scelte.
- `getSelectedValuesList():` Restituisce un oggetto List.

**JScrollPane**
È la classe che implementa la **barra di scorrimento**.
- Il metodo `setVisibleRowCount(int n)` nel caso della JList fissa un numero massimo di elementi visualizzabili per la lista.

**JComboBox**
È la classe che implementa una **lista di valori in discesa** in cui puoi scegliere un valore oppure modificarlo. L'evento generato è un **ActionEvent**.
- `addItem():` Aggiunge le voci proposte.
- `getSelectedItem():` Recupera la voce scelta/scritta.

**JTable**
È la classe che crea una **tabella**. Le JTable recuperano i dati da rappresentare tramite un **modello**, istanza di una classe che implementa l'interfaccia **TableModel**.

**TableModel**
Il TableModel mette a disposzione **metodi** per sapere il numero di righe/colonne, i dati contenuti in una cella, se le celle sono editabili...
- L'**AbstractTableModel** implementa la maggior parte dei metodi del TableModel e lascia al programmatore da implementare tre metodi.

**Metodi AbstractTableModel**
  - `public int getRowCount()`
  - `public int getColumnCount()`
  - `public Object getValueAt(int row, int column)`

**Modificare Dati Su JTable**
- `setValueAt():` Modifica i dati del modello.
- `fireTableDataChanged():` Evento che informa la tabella delle modifiche.

**Menù in Swing**
- **JMenu**: Rappresenta il menù.
- **JMenuBar**: Rappresenta la barra del menù.
- **JMenuItem**: Rappresenta una voce del menù.