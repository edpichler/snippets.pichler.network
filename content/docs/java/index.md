---
title: "Java"
weight: 10
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---
# Java

The strict rules of Java are a barrier of entrance for some developers which in the long term pays off removing problems that happens in most of languages, creating robust solutions and lowering the maintenance costs, which are the phase where most money is spent (90% of the sofware costs).

## AWT and Swing drag and drop in Java

``` java

public class Animal {

	private String name;
	private int age;

	public Animal() {
		name = "Name " + System.currentTimeMillis();
		age = (int) +System.currentTimeMillis();
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public int getAge() {
		return age;
	}

	public void setAge(int age) {
		this.age = age;
	}
}

```

```java
import javax.swing.JLabel;


public class JLabelAnimal extends JLabel{
	private Animal animal;

	public JLabelAnimal(Animal a){
		this.setAnimal(a);
	}

	public void setAnimal(Animal animal) {
		this.animal = animal;
		this.setText(animal.getName() + " - " + animal.getAge());
	}

	public Animal getAnimal() {
		return animal;
	}
}
```

``` java
import java.awt.Cursor;
import java.awt.EventQueue;
import java.awt.SystemColor;
import java.awt.datatransfer.DataFlavor;
import java.awt.datatransfer.Transferable;
import java.awt.datatransfer.UnsupportedFlavorException;
import java.awt.dnd.DnDConstants;
import java.awt.dnd.DragGestureEvent;
import java.awt.dnd.DragGestureListener;
import java.awt.dnd.DragSource;
import java.awt.dnd.DropTarget;
import java.awt.dnd.DropTargetAdapter;
import java.awt.dnd.DropTargetDropEvent;
import java.awt.dnd.DropTargetListener;
import java.io.IOException;

import javax.swing.JFrame;
import javax.swing.JPanel;
import javax.swing.border.EmptyBorder;

public class Main extends JFrame {

	private JPanel contentPane;
	private JPanel esquerda;
	private JPanel direita;
	private JLabelAnimal labelAnimal;
	
	DataFlavor dataFlavor = new DataFlavor(Animal.class,
			Animal.class.getSimpleName());

	/**
	 * Launch the application.
	 */
	public static void main(String[] args) {
		EventQueue.invokeLater(new Runnable() {
			public void run() {
				try {
					new Main().setVisible(true);
				} catch (Exception e) {
					e.printStackTrace();
				}
			}
		});
	}

	/**
	 * Create the frame.
	 */
	public Main() {
		setTitle("D'n'D example");
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setBounds(100, 100, 450, 300);
		contentPane = new JPanel();
		contentPane.setBorder(new EmptyBorder(5, 5, 5, 5));
		setContentPane(contentPane);
		contentPane.setLayout(null);

		esquerda = new JPanel();
		esquerda.setBackground(SystemColor.desktop);
		esquerda.setBounds(5, 5, 186, 263);
		contentPane.add(esquerda);

		labelAnimal = new JLabelAnimal(new Animal());
		esquerda.add(labelAnimal);

		direita = new JPanel();
		direita.setBackground(SystemColor.desktop);
		direita.setBounds(251, 5, 186, 263);
		contentPane.add(direita);

		init();
	}

	private void init() {
		DragSource ds = new DragSource();
		ds.createDefaultDragGestureRecognizer(labelAnimal,
				DnDConstants.ACTION_COPY, new DragGestureListImp());

		new MyDropTargetListImp(direita);
	}

	class TransferableAnimal implements Transferable {

		private Animal animal;

		public TransferableAnimal(Animal ani) {
			this.animal = ani;
		}

		@Override
		public DataFlavor[] getTransferDataFlavors() {
			return new DataFlavor[] { dataFlavor };
		}

		@Override
		public boolean isDataFlavorSupported(DataFlavor flavor) {
			return flavor.equals(dataFlavor);
		}

		@Override
		public Object getTransferData(DataFlavor flavor)
				throws UnsupportedFlavorException, IOException {

			if (flavor.equals(dataFlavor))
				return animal;
			else
				throw new UnsupportedFlavorException(flavor);
		}
	}

	class DragGestureListImp implements DragGestureListener {

		@Override
		public void dragGestureRecognized(DragGestureEvent event) {
			Cursor cursor = null;
			JLabelAnimal lblAnimal = (JLabelAnimal) event.getComponent();

			if (event.getDragAction() == DnDConstants.ACTION_COPY) {
				cursor = DragSource.DefaultCopyDrop;
			}
			Animal animal = lblAnimal.getAnimal();

			event.startDrag(cursor, new TransferableAnimal(animal));
		}
	}

	class MyDropTargetListImp extends DropTargetAdapter implements
			DropTargetListener {

		private DropTarget dropTarget;
		private JPanel panel;

		public MyDropTargetListImp(JPanel panel) {
			this.panel = panel;

			dropTarget = new DropTarget(panel, DnDConstants.ACTION_COPY, this,
					true, null);
		}

		public void drop(DropTargetDropEvent event) {
			try {
				Transferable tr = event.getTransferable();
				Animal an = (Animal) tr.getTransferData(dataFlavor);

				if (event.isDataFlavorSupported(dataFlavor)) {
					event.acceptDrop(DnDConstants.ACTION_COPY);
					this.panel.add(new JLabelAnimal(an));
					event.dropComplete(true);
					this.panel.validate();
					return;
				}
				event.rejectDrop();
			} catch (Exception e) {
				e.printStackTrace();
				event.rejectDrop();
			}
		}
	}
}
```
## Masking sensitive data in JSON using regex

``` java

static class JsonMasker {
    private static final Pattern JSON_PATTERN = Pattern.compile("(\"token\": *\")([^\"]+)*(\")",
                                                                Pattern.CASE_INSENSITIVE | Pattern.MULTILINE);

    private static String maskToken(final String message) {
        return JSON_PATTERN.matcher(message).replaceAll("$1*****$3");
    }
}
```