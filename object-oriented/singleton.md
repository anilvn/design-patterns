# Java Design Patterns: Singleton

## Table of Contents

| # | Question |
|---|----------|
| 1 | [What is the Singleton Design Pattern?](#what-is-the-singleton-design-pattern) |
| 2 | [How to implement a thread-safe Singleton in Java?](#how-to-implement-a-thread-safe-singleton-in-java) |
| 3 | [What are the advantages and disadvantages of Singleton Pattern?](#what-are-the-advantages-and-disadvantages-of-singleton-pattern) |
| 4 | [How to prevent Singleton pattern breaking with serialization?](#how-to-prevent-singleton-pattern-breaking-with-serialization) |

## What is the Singleton Design Pattern?

The Singleton pattern is a creational design pattern that ensures a class has only one instance and provides a global point of access to that instance.

## How to implement a thread-safe Singleton in Java?

Thread-safe Singleton implementations ensure that the singleton property is maintained even in multi-threaded environments.

Key approaches:
- **Eager Initialization**: Create instance at class loading time
- **Double-Checked Locking**: Check null condition twice with synchronization
- **Static Inner Class**: Uses JVM's class loading mechanism for thread safety
- **Enum-based**: Java ensures enum instances are created only once


```java
public class Singleton {

    // Volatile ensures changes to this variable are visible to all threads
    private static volatile Singleton instance;

    // Private constructor to prevent instantiation
    private Singleton() {
        System.out.println("Singleton instance created");
    }

    // Public method to provide global access point
    public static Singleton getInstance() {
        if (instance == null) { // First check (no locking)
            synchronized (Singleton.class) {
                if (instance == null) { // Second check (with locking)
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }

    
}
```

- also you can look for other singleton implementations like
  - `Static Inner Class (Bill Pugh Singleton)`
  - `Enum-based Singleton`

[Back to Top](#table-of-contents)

## What are the advantages and disadvantages of Singleton Pattern?

### Advantages

- **Controlled Access**: Provides a single global access point to the instance
- **Reduced Memory**: Only one instance is created, saving memory
- **Coordination**: Facilitates coordination between system components
- **Lazy Initialization**: Can defer creation until needed (in some implementations)
- **Configuration Management**: Good for managing application configuration

### Disadvantages

- **Global State**: Introduces global state which can make testing difficult
- **Thread Safety Issues**: Basic implementation isn't thread-safe
- **Difficulty in Unit Testing**: Hard to mock for testing
- **Tight Coupling**: Classes that use the singleton become coupled to it
- **Violation of Single Responsibility Principle**: The class manages its own lifecycle

Comparison with other patterns:

| Aspect | Singleton | Static Utility Class | Dependency Injection |
|--------|-----------|----------------------|----------------------|
| **State** | Can maintain state | Usually stateless | State controlled externally |
| **Inheritance** | Can be extended | Cannot be extended | Supports inheritance |
| **Testing** | Hard to mock | Hard to mock | Easy to mock |
| **Initialization** | Can be lazy or eager | Always eager | Managed externally |
| **Thread Safety** | Needs special handling | Thread safety per method | Handled externally |

[Back to Top](#table-of-contents)

## 🛡️ How to Protect Singleton from Reflection Attack?

- Add a static boolean flag (example: private static boolean instanceCreated = false;)
- Inside constructor, check if an instance already exists.
  - If yes, throw an exception (RuntimeException).
```java
public class Singleton {

    private static volatile Singleton instance;
    private static boolean instanceCreated = false; // 🚀 Important flag

    private Singleton() {
        if (instanceCreated) {
            throw new RuntimeException("Reflection is not allowed. Singleton instance already created!");
        }
        instanceCreated = true;
        System.out.println("Singleton instance created");
    }

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}

```

## How to prevent Singleton pattern breaking with serialization?

Serialization can break the Singleton pattern because deserialization creates a new instance. To prevent this:

Key techniques:
- **Implement readResolve method**: Returns the singleton instance during deserialization
- **Enum-based Singleton**: Java ensures serialization safety for enums

### Using readResolve Method

```java
import java.io.*;

// Make sure the class implements Serializable
public class Singleton implements Serializable {
    private static final long serialVersionUID = 1L;

    // Volatile ensures changes to this variable are visible to all threads
    private static volatile Singleton instance;

    // Private constructor to prevent instantiation
    private Singleton() {
        System.out.println("Singleton instance created");
    }

    // Public method to provide global access point
    public static Singleton getInstance() {
        if (instance == null) { // First check (no locking)
            synchronized (Singleton.class) {
                if (instance == null) { // Second check (with locking)
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }

    // Key part to fix serialization breaking Singleton
    protected Object readResolve() {
        return getInstance();
    }

    // main method to test serialization
    public static void main(String[] args) throws Exception {
        Singleton s1 = Singleton.getInstance();

        // Serialize to a file
        ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream("file.ser"));
        out.writeObject(s1);
        out.close();

        // Deserialize from the file
        ObjectInputStream in = new ObjectInputStream(new FileInputStream("file.ser"));
        Singleton s2 = (Singleton) in.readObject();
        in.close();

        System.out.println("s1 hashCode: " + s1.hashCode());
        System.out.println("s2 hashCode: " + s2.hashCode());
        System.out.println("Are they same object? " + (s1 == s2));
    }
}

```
### outupt:
```yaml
Singleton instance created
s1 hashCode: 12345678
s2 hashCode: 12345678
Are they same object? true

```

### Enum-based Singleton (Serialization-safe)

```java
public enum EnumSingleton {
    INSTANCE;
    
    private String data;
    
    public String getData() {
        return data;
    }
    
    public void setData(String data) {
        this.data = data;
    }

    public void doSomething() {
        System.out.println("Doing something...");
    }

}

```

### 🚀 Why Enum Singleton is Automatically Serialization-safe?

When Java serializes an Enum:

- It **doesn't serialize the fields** like a normal object.
- It **serializes only the name** of the Enum constant (e.g., `"INSTANCE"`).
- During **deserialization**, Java **internally uses the same object** from JVM memory — it **does not create a new object**.

### ✨ In short:
> Java guarantees that for Enums, **deserialization returns the same instance** that was already loaded!

👉 **No need for `readResolve()`!**  
👉 **No need to manually code anything extra!**


### 📋 Enum Singleton Advantages

| Problem               | Enum automatically handles? |
|------------------------|:----------------------------:|
| Serialization issue    | ✅ Solved                     |
| Reflection attack      | ✅ Blocked                    |
| Thread safety          | ✅ Guaranteed                 |
| Easy syntax            | ✅ Yes                        |
---

### 🛡️ First, What is a Reflection Attack on Singleton?

Normally, in Java, using **Reflection**, you can access a private constructor and create a new instance, breaking Singleton!

#### Example with normal Singleton:

```java
Constructor<Singleton> constructor = Singleton.class.getDeclaredConstructor();
constructor.setAccessible(true);
Singleton newInstance = constructor.newInstance(); // ❌ Creates new object
```

✅ **Boom!** Singleton is broken — you now have two instances.

---

### 🔥 But... why can't this happen with an Enum Singleton?

Because:

- Java **strictly prevents Reflection** from creating new Enum instances.
- Internally, when you try to use Reflection to create an enum object, **Java throws an exception immediately**.
- This behavior is **built into Java's `Enum` class**.

---

### ⚙️ Proof with Code:
**Trying Reflection on Enum Singleton:**

```java
import java.lang.reflect.Constructor;

public class EnumReflectionTest {
    public static void main(String[] args) {
        try {
            Constructor<Singleton> constructor = Singleton.class.getDeclaredConstructor();
            constructor.setAccessible(true);
            Singleton instance = constructor.newInstance();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

enum Singleton {
    INSTANCE;
}
```

### Output:

```text
java.lang.NoSuchMethodException: Singleton.<init>()
```
or sometimes:

```text
java.lang.IllegalArgumentException: Cannot reflectively create enum objects
```

✅ **Reflection fails.**

---

### 🧠 Why exactly?

- When the JVM loads an enum, it **automatically injects extra protection**.
- All Enum constructors are **implicitly private**, and **Java does not allow reflective instantiation** of Enums.
- Methods like `Class.newInstance()` and `Constructor.newInstance()` **check if the class is an enum** and **throw `IllegalArgumentException`** if you try to instantiate it.


[Back to Top](#table-of-contents)