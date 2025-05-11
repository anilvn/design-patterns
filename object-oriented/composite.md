
# Composite Design Pattern

> **Composite Design Pattern** is a structural design pattern that lets you compose objects into tree structures to represent part-whole hierarchies. It allows individual objects (leaf nodes) and groups of objects (composite nodes) to be treated uniformly through a common interface.

---

### **Key Points**:
- It **combines objects** into tree-like structures.
- **Leaf** objects represent **individual items**.
- **Composite** objects represent **groups** (they can hold leaves or other composites).
- Clients **treat leaf and composite objects uniformly**, meaning they don’t need to know if they’re working with a single object or a group.

---

### **Example in simple words**:
- A **File** is a single object (Leaf).
- A **Folder** can contain Files and other Folders (Composite).
- Whether you deal with a single File or a Folder full of Files, **you can call the same method** (like `showDetails()`).

### Here's a Java example demonstrating the **Composite Design Pattern** using a **file and folder** structure.  

---

### **Java Implementation**
```java
import java.util.ArrayList;
import java.util.List;

// Component
interface FileSystem {
    void showDetails();
}

// Leaf
class File implements FileSystem {
    private String name;

    public File(String name) {
        this.name = name;
    }

    @Override
    public void showDetails() {
        System.out.println("File: " + name);
    }
}
```

```java
// Composite
class Folder implements FileSystem {
    private String name;
    private List<FileSystem> items = new ArrayList<>();

    public Folder(String name) {
        this.name = name;
    }

    public void add(FileSystem item) {
        items.add(item);
    }

    public void remove(FileSystem item) {
        items.remove(item);
    }

    @Override
    public void showDetails() {
        System.out.println("Folder: " + name);
        for (FileSystem item : items) {
            item.showDetails();
        }
    }
}
```

```java
// Client
public class CompositePatternExample {
    public static void main(String[] args) {
        // Creating files
        File file1 = new File("document.pdf");
        File file2 = new File("photo.jpg");
        File file3 = new File("video.mp4");

        // Creating folders
        Folder rootFolder = new Folder("Root");
        Folder subFolder1 = new Folder("Pictures");
        Folder subFolder2 = new Folder("Videos");

        // Adding files to folders
        subFolder1.add(file2);
        subFolder2.add(file3);

        rootFolder.add(file1);
        rootFolder.add(subFolder1);
        rootFolder.add(subFolder2);

        // Displaying the hierarchy
        rootFolder.showDetails();
    }
}
```

---

### **Output**
```
Folder: Root
File: document.pdf
Folder: Pictures
File: photo.jpg
Folder: Videos
File: video.mp4
```

This structure allows for easy expansion by adding more files and folders dynamically while maintaining a unified interface. 🚀
