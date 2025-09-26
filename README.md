# a-text-based-notes-manager-with-file-read-write.
import java.io.*;
import java.util.*;

public class NotesManager {
    private static final String FILE_NAME = "notes.txt";
    private static Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) {
        int choice;

        do {
            printMenu();
            System.out.print("Enter your choice: ");
            choice = Integer.parseInt(scanner.nextLine());

            switch (choice) {
                case 1 -> addNote();
                case 2 -> viewNotes();
                case 3 -> deleteNote();
                case 4 -> System.out.println("Exiting. Goodbye!");
                default -> System.out.println("Invalid choice. Try again.");
            }

        } while (choice != 4);
    }

    private static void printMenu() {
        System.out.println("\n==== Notes Manager ====");
        System.out.println("1. Add Note");
        System.out.println("2. View Notes");
        System.out.println("3. Delete Note");
        System.out.println("4. Exit");
    }

    private static void addNote() {
        System.out.print("Enter your note: ");
        String note = scanner.nextLine();

        try (FileWriter fw = new FileWriter(FILE_NAME, true);
             BufferedWriter bw = new BufferedWriter(fw)) {
            bw.write(note);
            bw.newLine();
            System.out.println("Note added successfully.");
        } catch (IOException e) {
            System.out.println("Error writing to file.");
        }
    }

    private static void viewNotes() {
        File file = new File(FILE_NAME);

        if (!file.exists()) {
            System.out.println("No notes found.");
            return;
        }

        try (BufferedReader br = new BufferedReader(new FileReader(FILE_NAME))) {
            String line;
            int count = 1;
            System.out.println("\nYour Notes:");
            while ((line = br.readLine()) != null) {
                System.out.println(count++ + ". " + line);
            }
        } catch (IOException e) {
            System.out.println("Error reading file.");
        }
    }

    private static void deleteNote() {
        List<String> notes = new ArrayList<>();

        try (BufferedReader br = new BufferedReader(new FileReader(FILE_NAME))) {
            String line;
            while ((line = br.readLine()) != null) {
                notes.add(line);
            }
        } catch (IOException e) {
            System.out.println("Error reading file.");
            return;
        }

        if (notes.isEmpty()) {
            System.out.println("No notes to delete.");
            return;
        }

        System.out.println("Select the number of the note to delete:");
        for (int i = 0; i < notes.size(); i++) {
            System.out.println((i + 1) + ". " + notes.get(i));
        }

        System.out.print("Enter number: ");
        int index;
        try {
            index = Integer.parseInt(scanner.nextLine());
            if (index < 1 || index > notes.size()) {
                System.out.println("Invalid note number.");
                return;
            }
        } catch (NumberFormatException e) {
            System.out.println("Invalid input.");
            return;
        }

        notes.remove(index - 1);

        try (BufferedWriter bw = new BufferedWriter(new FileWriter(FILE_NAME))) {
            for (String note : notes) {
                bw.write(note);
                bw.newLine();
            }
            System.out.println("Note deleted.");
        } catch (IOException e) {
            System.out.println("Error writing to file.");
        }



        
README.MD


# 📝 Text-Based Notes Manager (Java)

This is a simple **Command-Line Interface (CLI) Notes Manager** written in **Java**. It allows users to:

- Add new notes
- View existing notes
- Delete notes by number
- Store and manage notes using file read/write operations
  
## Features

- CLI Menu Interface
- File-based storage using `notes.txt`
- Notes are persistent across sessions
- Basic error handling
- Clean and modular code
  
## 💻 Technologies & Concepts Used

### Features:
- **`FileReader`, `FileWriter`, `BufferedReader`, `BufferedWriter`**  
  Used to perform file input and output operations for saving and loading notes.

- **`Scanner` class**  
  Used for reading user input from the console.

- **`ArrayList`**  
  Used to store notes temporarily in memory while reading from or writing to the file (especially during deletion).

- **`try-with-resources`**  
  Ensures all I/O streams are properly closed after operations, preventing memory/resource leaks.

- **Java 8+ enhanced features**  
  Uses lambda expressions (in the form of `switch-case ->`) for cleaner control structures.
## How It Works

1. **Add Note**
- Prompts the user to enter a note.
- Appends it to the `notes.txt` file.

2. **View Notes**
- Reads all lines from `notes.txt` and displays them with index numbers.

3. **Delete Note**
- Loads all notes into an `ArrayList`.
- Prompts the user to select a note by index to delete.
- Rewrites the entire file excluding the deleted note.

4. **Exit**
- Cleanly terminates the program.

    }
}
