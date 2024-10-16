

---

# Creating a Simple Text Editor with Java Swing

## Introduction
Today, we'll create a simple text editor application using Java Swing. This text editor will allow us to create, open, save, print, and manipulate text files. We'll also implement basic editing functions like cut, copy, paste, and select all.

## Step 1: Setting Up the Project
First, let's set up our Java project. Open your favorite Java IDE (like IntelliJ IDEA, Eclipse, or NetBeans) and create a new Java project.

## Step 2: Importing Necessary Libraries
Let's start by importing the necessary libraries for our application. These libraries include classes for GUI components, file handling, and event handling.

```java
import java.awt.*;
import java.awt.event.*;
import java.awt.print.PrinterException;
import java.io.*;
import javax.swing.*;
import javax.swing.filechooser.FileNameExtensionFilter;
```

Here, we import all the required classes for GUI, file operations, and event handling.

## Step 3: Creating the Main Class
Next, let's create the main class for our text editor. This class will extend `JFrame` and implement `ActionListener`.

```java
public class TextEditor extends JFrame implements ActionListener {
    // Menu bar
    private JMenuBar menuBar;
    // Menus
    private JMenu fileMenu, editMenu, saveAndSubmitMenu;
    // Menu items for "File" menu
    private JMenuItem newFileItem, openFileItem, saveFileItem, printFileItem, exitItem;
    // Menu items for "Edit" menu
    private JMenuItem cutItem, copyItem, pasteItem, selectAllItem;
    // Menu item for "Save and Submit" menu
    private JMenuItem saveAndSubmitItem;
    // Text area for the editor
    private JTextArea textArea;

    public static void main(String[] args) throws Exception {
        UIManager.setLookAndFeel(UIManager.getSystemLookAndFeelClassName()); // Set the look and feel of the UI to the system's look and feel
        new TextEditor().setVisible(true); // Create an instance of the text editor and make it visible
    }
}
```
## Step 4 : Intitalising in the main funtion
To view its functionality
```java
    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            try {
                UIManager.setLookAndFeel(UIManager.getSystemLookAndFeelClassName()); // Set the look and feel of the UI to the system's look and feel
            } catch (Exception ex) {
                ex.printStackTrace(); // Print the stack trace for any exceptions
            }
            new TextEditor().setVisible(true); // Create an instance of the text editor and make it visible
        });
    }

```
## Step 5: Setting Up the Constructor
Now, let's set up the constructor to initialize the GUI components and layout.

```java
public TextEditor() {
    // Set the title of the frame
    setTitle("Text Editor");
    // Set the size and position of the frame
    setBounds(100, 100, 800, 600);
    // Ensure the application exits when the frame is closed
    setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

    // Initialize the menu bar and menus
    menuBar = new JMenuBar();
    fileMenu = new JMenu("File");
    editMenu = new JMenu("Edit");
    saveAndSubmitMenu = new JMenu("Save and Submit");

    // Create and add menu items to the "File" menu
    newFileItem = createMenuItem("New", fileMenu);
    openFileItem = createMenuItem("Open", fileMenu);
    saveFileItem = createMenuItem("Save", fileMenu);
    printFileItem = createMenuItem("Print", fileMenu);
    exitItem = createMenuItem("Exit", fileMenu);

    // Create and add menu items to the "Edit" menu
    cutItem = createMenuItem("Cut", editMenu);
    copyItem = createMenuItem("Copy", editMenu);
    pasteItem = createMenuItem("Paste", editMenu);
    selectAllItem = createMenuItem("Select All", editMenu);

    // Create and add menu item to the "Save and Submit" menu
    saveAndSubmitItem = createMenuItem("Save and Exit", saveAndSubmitMenu);

    // Set the menu bar for the frame
    setJMenuBar(menuBar);
    // Add menus to the menu bar
    menuBar.add(fileMenu);
    menuBar.add(editMenu);
    menuBar.add(saveAndSubmitMenu);

    // Initialize the text area and add it to a scroll pane
    textArea = new JTextArea();
    JScrollPane scrollPane = new JScrollPane(textArea);
    // Configure the scroll pane
    scrollPane.setHorizontalScrollBarPolicy(JScrollPane.HORIZONTAL_SCROLLBAR_NEVER);
    scrollPane.setVerticalScrollBarPolicy(JScrollPane.VERTICAL_SCROLLBAR_AS_NEEDED);
    scrollPane.setBorder(BorderFactory.createEmptyBorder());
    // Add the scroll pane to the frame
    add(scrollPane);

    // Set font and wrapping properties for the text area
    textArea.setFont(new Font(Font.SANS_SERIF, Font.PLAIN, 20));
    textArea.setLineWrap(true);
    textArea.setWrapStyleWord(true);
}
```

## Step 6: Creating Menu Items
We'll create a helper method to create menu items and add them to menus.

```java
private JMenuItem createMenuItem(String name, JMenu menu) {
    JMenuItem menuItem = new JMenuItem(name);
    menuItem.addActionListener(this); // Set this class as the action listener for the menu item
    menu.add(menuItem); // Add the menu item to the menu
    return menuItem;
}
```

## Step 7: Handling Menu Actions
Now, let's handle the actions for each menu item.

```java
@Override
public void actionPerformed(ActionEvent e) {
    String command = e.getActionCommand(); // Get the action command

    switch (command) {
        case "New":
            textArea.setText(null); // Clear the text area
            break;
        case "Open":
            openFile(); // Call method to open a file
            break;
        case "Save":
        case "Save and Exit":
            saveFile(); // Call method to save the file
            if (command.equals("Save and Exit")) {
                System.exit(0); // Exit the application if the command is "Save and Exit"
            }
            break;
        case "Print":
            printFile(); // Call method to print the file
            break;
        case "Exit":
            System.exit(0); // Exit the application
            break;
        case "Cut":
            textArea.cut(); // Cut the selected text
            break;
        case "Copy":
            textArea.copy(); // Copy the selected text
            break;
        case "Paste":
            textArea.paste(); // Paste the clipboard text
            break;
        case "Select All":
            textArea.selectAll(); // Select all text in the text area
            break;
    }
}
```

## Step 8: Implementing File Operations
Next, let's implement the methods for file operations.

```java
// Method to open a file and read its content into the text area
private void openFile() {
    JFileChooser fileChooser = new JFileChooser(); // Create a file chooser dialog
    FileNameExtensionFilter filter = new FileNameExtensionFilter("Text Files (.txt)", "txt");
    fileChooser.setFileFilter(filter); // Set the file filter to accept only .txt files
    int action = fileChooser.showOpenDialog(null); // Show the open dialog

    if (action == JFileChooser.APPROVE_OPTION) {
        try (BufferedReader reader = new BufferedReader(new FileReader(fileChooser.getSelectedFile()))) {
            textArea.read(reader, null); // Read the file content into the text area
        } catch (IOException e) {
            e.printStackTrace(); // Print the stack trace for any IOException
        }
    }
}

// Method to save the content of the text area to a file
private void saveFile() {
    JFileChooser fileChooser = new JFileChooser(); // Create a file chooser dialog
    FileNameExtensionFilter filter = new FileNameExtensionFilter("Text Files (.txt)", "txt");
    fileChooser.setFileFilter(filter); // Set the file filter to accept only .txt files
    int action = fileChooser.showSaveDialog(null); // Show the save dialog

    if (action == JFileChooser.APPROVE_OPTION) {
        String fileName = fileChooser.getSelectedFile().getAbsolutePath();
        if (!fileName.endsWith(".txt")) {
            fileName += ".txt"; // Ensure the file name ends with .txt
        }
        try (BufferedWriter writer = new BufferedWriter(new FileWriter(fileName))) {
            textArea.write(writer); // Write the content of the text area to the file
        } catch (IOException e) {
            e.printStackTrace(); // Print the stack trace for any IOException
        }
    }
}

// Method to print the content of the text area
private void printFile() {
    try {
        textArea.print(); // Print the content of the text area
    } catch (PrinterException e) {
        e.printStackTrace(); // Print the stack trace for any PrinterException
    }
}
```


## Conclusion
That's it! We've built a simple text editor using Java Swing. You can now create, open, save, print, and edit text files using this application. Feel free to expand this project by adding more features or improving the UI.

---

This Markdown script provides a clear, step-by-step guide to creating a text editor application in Java Swing, making it easy to follow and teach others.
