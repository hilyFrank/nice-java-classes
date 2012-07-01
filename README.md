nice-java-classes
=================
/*
  This is an implementation of a textpad [it's similar to notepad buut with little options] using GUI, it has functionalities like new open save as, save and font editing...

*/
package frank;

import javax.swing.*;

import java.awt.*;
import java.awt.event.*;
import java.io.*;




public class TextPad extends JFrame implements ActionListener{
  JTextArea textPad;	                                   	// text area
	JMenu fileOption,fontOption, subMenu,changeFontType;	  // Menus for file, font type, font colour, 
	JMenuItem newItem, openItem,saveItem,saveAsItem,exitItem;// save, open, save as and exit sub menues under file menu
	JMenuItem  changeFontColor, changeFontSize;              // the menues for changing the font color and size under the font menu
	JFileChooser jc = new JFileChooser();                   // File chooser obeject used in saving and opening files
	File fileName;			                                    // file name to be opened or saved
	String title = "Untitled";
	Color fontColor = Color.black;                         // default font color
  String fontType = "Verdana";                            // default font type
	Font font;			                                    	  // font object
  int fontSize = 12;		                              	// font size of the font object
	
	public TextPad(){
		setTitle(title +" - TextPad");
		setSize(650,600);
		
		setLayout(new FlowLayout());
		// File Menu 
		fileOption = new JMenu("File");
		fileOption.setMnemonic(KeyEvent.VK_F);
		
		// New option menu
  	newItem = new JMenuItem("New");
		// activating the new option with ctrl + n
		newItem.setAccelerator(KeyStroke.getKeyStroke(KeyEvent.VK_N, ActionEvent.CTRL_MASK));
		
		// create imageicon for new
		ImageIcon newIcon = new ImageIcon("New.gif");
		
		newItem.setIcon(newIcon);
		newItem.addActionListener(new ActionListener(){
			public void actionPerformed(ActionEvent e){
				textPad.setText("");
				title = "Untitled";
				setTitle(title+ " - TextPad");
			}
			
		});
		
		// attaching the new option onto the file menu
		fileOption.add(newItem);
		
		// Open option menu
		openItem = new JMenuItem("Open");
		//openItem.setMnemonic(KeyEvent.VK_O);
		
		// activating the open option with ctrl + o
		openItem.setAccelerator(KeyStroke.getKeyStroke(KeyEvent.VK_O, ActionEvent.CTRL_MASK));
		
		// Creating an imageicon object for the open menu
		ImageIcon openIcon = new ImageIcon("Open16.gif");
		
		// setting the icon for open menu
		openItem.setIcon(openIcon);
		openItem.addActionListener(this);
		fileOption.add(openItem);
		
		// Save option menu
		saveItem = new JMenuItem("Save");
		
		// activating the save option with ctrl + s
		saveItem.setAccelerator(KeyStroke.getKeyStroke(KeyEvent.VK_S, ActionEvent.CTRL_MASK));
		
		// Creating an imageicon object for the save menu
		ImageIcon saveIcon = new ImageIcon("Save16.gif");
		
		// setting the icon for save menu
		saveItem.setIcon(saveIcon);
		//saveItem.setMnemonic(KeyEvent.VK_S);
		saveItem.addActionListener(this);
		fileOption.add(saveItem);
		
		// Save As option menu
		saveAsItem = new JMenuItem("SaveAs");
		//saveAsItem.setMnemonic(KeyEvent.VK_S);
		saveAsItem.addActionListener(this);
		fileOption.add(saveAsItem);
		
		//Exit option menu
		exitItem = new JMenuItem("Ëxit");
		exitItem.setMnemonic(KeyEvent.VK_X);
		exitItem.addActionListener(this);
		fileOption.add(exitItem);
		
		// Font option
		
		fontOption = new JMenu("Font");
		
		//Change font color
		changeFontColor = new JMenuItem("Font color");
		changeFontColor.addActionListener(this);
		fontOption.add(changeFontColor);
		
		// change font type
		changeFontType = new JMenu("Font Type");
		JMenuItem verdana = new JMenuItem("Verdana");
		verdana.addActionListener(new ActionListener(){
			public void actionPerformed(ActionEvent e){
				fontType = "Verdana";
				font = new Font(fontType, Font.PLAIN,fontSize);
				textPad.setFont(font);
			}
		});
		changeFontType.add(verdana);
		
		JMenuItem monotypeCorsiva = new JMenuItem("Monotype Corsiva");
		monotypeCorsiva.addActionListener(new ActionListener(){
			public void actionPerformed(ActionEvent e){
				fontType = "Monotype Corsiva";
				font = new Font(fontType, Font.PLAIN,fontSize);
				textPad.setFont(font);
			}
		});
		changeFontType.add(monotypeCorsiva);
		
		JMenuItem sansSerif = new JMenuItem("SansSerif");
		sansSerif.addActionListener(new ActionListener(){
			public void actionPerformed(ActionEvent e){
				fontType = "SansSerif";
				font = new Font(fontType, Font.PLAIN,fontSize);
				textPad.setFont(font);
			}
		});
		changeFontType.add(sansSerif);
		
		fontOption.add(changeFontType);
		
		// changing the font size
		changeFontSize = new JMenuItem("Font Size");
		changeFontSize.addActionListener(new ActionListener(){
			public void actionPerformed(ActionEvent e){
					String size = "";
					
					// Keep nagging the user until the right size is enter ie [non-empty, and 11 <= size <= 36]
					while (size.equals("") || Integer.parseInt(size) < 11 || Integer.parseInt(size) > 36){
						size = JOptionPane.showInputDialog(null, "Enter the font size [should be btw 11 and 36]");
					}
					// converting size to integer
					fontSize = Integer.parseInt(size);
					font = new Font(fontType, Font.PLAIN,fontSize);
					
					// reset the font info
					textPad.setFont(font);
			}
		});
		
		// attaching the font size sub menu onto the font menu
		fontOption.add(changeFontSize);
		
		// Creating a menu bar for the menu objects 
		JMenuBar bar = new JMenuBar();
		 
    // Attaching the menus on the menu bar 
		bar.add(fileOption);
		bar.add(fontOption);
		setJMenuBar(bar);
		
		// Creating the attributes of the textArea - textPad
		textPad = new JTextArea(45,40);
		font = new Font(fontType, Font.PLAIN, 13);
		textPad.setFont(font);
		textPad.setForeground(fontColor);
	
		
		//Enveloping the textArea in a scroll pane
		JScrollPane padPane = new JScrollPane(textPad);

		// setting the scrolls policy
		padPane.setHorizontalScrollBarPolicy(JScrollPane.HORIZONTAL_SCROLLBAR_AS_NEEDED);
		padPane.setVerticalScrollBarPolicy(JScrollPane.VERTICAL_SCROLLBAR_AS_NEEDED);

    // adding the pane onto the Frame
		add(padPane);

		setVisible(true);
	}

	public static void main(String args[]){
		TextPad scrolledFrame = new TextPad();
		
	}
	
	public void actionPerformed(ActionEvent e){
		String actionCommand = e.getActionCommand();
		if(actionCommand.equals("Open")){
			int returnVal = jc.showOpenDialog(this);
			if(returnVal == JFileChooser.APPROVE_OPTION){
				System.out.println("You chose to open this file: " + jc.getSelectedFile().getName());
				fileName = jc.getSelectedFile();
				
				// Opening a file for reading
				try{
					BufferedReader read = new BufferedReader(new FileReader(fileName));
					String lineRead;
					while((lineRead = read.readLine()) != null){
						textPad.append(lineRead);
					}
				}catch(IOException e1){
					System.out.println("An error occurred while opening this file");
					System.exit(0);
				}
			}
			
			
		}else if(actionCommand.equals("Save")){
			// First time saving
			if(title.equals("Untitled")){
				try {
					initialSave();
				} catch (IOException e1) {
					
					e1.printStackTrace();
				}
					
				}else{
					// Opening the new file for writing
					try{
						BufferedWriter toFile = new BufferedWriter(new FileWriter(fileName));
						toFile.write(textPad.getText());
						toFile.close();
					}catch(IOException e1){
						System.out.println("An error occurred while opening this file");
						System.exit(0);
					}
				}
		}else if(actionCommand.equals("SaveAs")){
			try {
				initialSave();
			} catch (IOException e1) {
				
				e1.printStackTrace();
			}
      
      // changing the color of the text
		}else if(actionCommand.equals("Font color")){
			fontColor = JColorChooser.showDialog(this, "Choose a font color", fontColor);
			textPad.setForeground(fontColor);
			
		}
		else{
			dispose();
		}
		
		}
	
  // does the save as operation
	public void initialSave() throws IOException{
		int returnVal = jc.showSaveDialog(this);
		BufferedWriter toFile = null;
		if(returnVal == JFileChooser.APPROVE_OPTION){
			fileName = jc.getSelectedFile();
			title = fileName.getName();
      
      // setting the title of the pad
			setTitle( title + "  -TextPad");
			
			// Opening the new file for writing
			try{
				toFile = new BufferedWriter(new FileWriter(fileName));
				toFile.write(textPad.getText());
				toFile.close();
			}catch(IOException e1){
				System.out.println("An error occurred while opening this file");
				System.exit(0);
			}finally{
				toFile.close();
			}
		}		
	}

}
