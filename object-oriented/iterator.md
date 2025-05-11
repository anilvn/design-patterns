# Iterator Pattern

### ✅ **Definition of the Iterator Design Pattern**

**Iterator Design Pattern** is a **behavioral design pattern** that provides a way to access the elements of a collection (such as a list or array) **sequentially without exposing the underlying structure** of the collection.

---

### 🔧 **Key Concepts**
- **Iterator**: An object that enables traversal through a collection.
- **Aggregate (or Collection)**: An interface that defines a method to create an iterator.
- **Concrete Iterator**: Implements the traversal logic (e.g., forward, reverse).
- **Concrete Aggregate**: Implements the method to return an instance of the iterator.

---

### 🧠 **Why Use It?**
- Decouples the iteration logic from the collection.
- Allows multiple traversal algorithms (e.g., forward, backward).
- Makes code cleaner, more modular, and easier to test.

---


- **`ReverseBookIterator`** – Iterates over the collection in reverse order.  
- **`BookCollectionIterator`** – The original forward iterator.  

Both iterators will be tested in the `Client` class to show their different behaviors.  

---

### **Updated Code with Two Iterators**
```java
import java.util.Arrays;
import java.util.Iterator;
import java.util.List;

// Book Class (Entity)
class Book {
    private int price;
    private String bookName;

    public Book(int price, String bookName) {
        this.price = price;
        this.bookName = bookName;
    }

    public int getPrice() {
        return price;
    }

    public String getBookName() {
        return bookName;
    }
}

// Iterator Interface
interface BookIterator extends Iterator<Book> {
}

// Forward Iterator - Iterates from first to last
class BookCollectionIterator implements BookIterator {
    private List<Book> books;
    private int index = 0;

    public BookCollectionIterator(List<Book> books) {
        this.books = books;
    }

    @Override
    public boolean hasNext() {
        return index < books.size();
    }

    @Override
    public Book next() {
        return books.get(index++);
    }
}

// Reverse Iterator - Iterates from last to first
class ReverseBookIterator implements BookIterator {
    private List<Book> books;
    private int index;

    public ReverseBookIterator(List<Book> books) {
        this.books = books;
        this.index = books.size() - 1;
    }

    @Override
    public boolean hasNext() {
        return index >= 0;
    }

    @Override
    public Book next() {
        return books.get(index--);
    }
}

// Aggregate Interface
interface Aggregate {
    Iterator<Book> createIterator();
    Iterator<Book> createReverseIterator();
}

// Library Class (Concrete Collection)
class Library implements Aggregate {
    private List<Book> booksList;

    public Library(List<Book> booksList) {
        this.booksList = booksList;
    }

    @Override
    public Iterator<Book> createIterator() {
        return new BookCollectionIterator(booksList);
    }

    @Override
    public Iterator<Book> createReverseIterator() {
        return new ReverseBookIterator(booksList);
    }
}

// Client Class to Test Different Iterators
public class Client {
    public static void main(String[] args) {
        List<Book> booksList = Arrays.asList(
                new Book(100, "Maths"),
                new Book(200, "Science"),
                new Book(300, "GK"),
                new Book(400, "Drawing")
        );

        Library lib = new Library(booksList);

        // Using Forward Iterator
        System.out.println("Iterating in Forward Order:");
        Iterator<Book> forwardIterator = lib.createIterator();
        while (forwardIterator.hasNext()) {
            Book book = forwardIterator.next();
            System.out.println("Book: " + book.getBookName() + " | Price: " + book.getPrice());
        }

        // Using Reverse Iterator
        System.out.println("\nIterating in Reverse Order:");
        Iterator<Book> reverseIterator = lib.createReverseIterator();
        while (reverseIterator.hasNext()) {
            Book book = reverseIterator.next();
            System.out.println("Book: " + book.getBookName() + " | Price: " + book.getPrice());
        }
    }
}
```

---

### **Output**
```
Iterating in Forward Order:
Book: Maths | Price: 100
Book: Science | Price: 200
Book: GK | Price: 300
Book: Drawing | Price: 400

Iterating in Reverse Order:
Book: Drawing | Price: 400
Book: GK | Price: 300
Book: Science | Price: 200
Book: Maths | Price: 100
```

---

### **Explanation**
1. **`BookCollectionIterator`** → Iterates in forward order (first to last).  
2. **`ReverseBookIterator`** → Iterates in reverse order (last to first).  
3. **`Library`** now supports both types of iterators (`createIterator()` for normal, `createReverseIterator()` for reverse).  
4. **`Client`** demonstrates both iterators to show their different behaviors.
