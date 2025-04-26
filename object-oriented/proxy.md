
# Proxy Design Pattern

### 🎯 Best Definition for Proxy Pattern:
> **The Proxy Pattern** is a structural design pattern that provides a **substitute or placeholder** for another object to control access to it.  
> The proxy can add additional behavior like **lazy loading**, **access control**, **logging**, or **caching** without changing the original object's code.

- **Key Point:**  
  It acts as a **middleman** between the client and the real object.

---

### 🎯 Interviewer Questions You Can Expect on Proxy Pattern:

1. **Why would you use a Proxy Pattern?**
   - (Control expensive resource usage, security access, remote communication, lazy initialization.)

2. **Different types of Proxies?**
   - **Virtual Proxy** (lazy initialization)  
   - **Protection Proxy** (access control)  
   - **Remote Proxy** (interacts with remote objects)  
   - **Cache Proxy** (caching results)

3. **How is Proxy different from Decorator?**
   - (Decorator **adds behavior**, Proxy **controls access**.)

4. **Can you give a real-world example?**
   - (Example: Image loading in a web app — real image loads only when needed.)

5. **Can Proxy and Singleton work together?**
   - (Yes, a proxy could be singleton if you want only one access point.)

6. **What are the pros and cons of Proxy Pattern?**
   - (Pros: control, security, optimization. Cons: extra layer of complexity.)

---

##  **Proxy Design Pattern example** 

- If **command is `READ`**, **any user** (admin or normal user) can execute.
- If **command is `CREATE` or `DELETE`**, then **only ADMIN** is allowed.
- If **not admin**, throw an exception like **"Access Denied"**.
- After check passes, **delegate** the request to real implementation.

Let’s quickly build this properly.

---

### **1. Command Interface**

```java
package LowLevelDesign.DesignPattern;

public interface CommandExecutor {
    void executeCommand(String command, String userRole) throws Exception;
}
```

---

### **2. Real Command Executor**

```java
package LowLevelDesign.DesignPattern;

public class CommandExecutorImpl implements CommandExecutor {

    @Override
    public void executeCommand(String command, String userRole) throws Exception {
        System.out.println("Executing command: " + command);
    }
}
```

---

### **3. Proxy for CommandExecutor**

```java
package LowLevelDesign.DesignPattern;

public class CommandExecutorProxy implements CommandExecutor {

    private CommandExecutor realExecutor;

    public CommandExecutorProxy() {
        this.realExecutor = new CommandExecutorImpl();
    }

    @Override
    public void executeCommand(String command, String userRole) throws Exception {
        String cmd = command.toUpperCase();

        if (cmd.startsWith("READ")) {
            realExecutor.executeCommand(command, userRole);
        } else if ((cmd.startsWith("CREATE") || cmd.startsWith("DELETE")) && userRole.equalsIgnoreCase("ADMIN")) {
            realExecutor.executeCommand(command, userRole);
        } else {
            throw new Exception("Access Denied: Only ADMIN can perform " + command + " operations.");
        }
    }
}
```

---

### **4. Main Class**

```java
package LowLevelDesign.DesignPattern;

public class ProxyDesignPatternDemo {
    public static void main(String[] args) {
        try {
            CommandExecutor userExecutor = new CommandExecutorProxy();

            System.out.println("User trying to READ...");
            userExecutor.executeCommand("READ EMPLOYEE DATA", "USER"); // Allowed

            System.out.println("User trying to CREATE...");
            userExecutor.executeCommand("CREATE EMPLOYEE", "USER");    // Should throw exception

        } catch (Exception e) {
            System.out.println(e.getMessage());
        }

        try {
            CommandExecutor adminExecutor = new CommandExecutorProxy();

            System.out.println("\nAdmin trying to CREATE...");
            adminExecutor.executeCommand("CREATE EMPLOYEE", "ADMIN");  // Allowed

            System.out.println("Admin trying to DELETE...");
            adminExecutor.executeCommand("DELETE EMPLOYEE", "ADMIN");  // Allowed

        } catch (Exception e) {
            System.out.println(e.getMessage());
        }
    }
}
```

---

### **Output**
```
User trying to READ...
Executing command: READ EMPLOYEE DATA
User trying to CREATE...
Access Denied: Only ADMIN can perform CREATE EMPLOYEE operations.

Admin trying to CREATE...
Executing command: CREATE EMPLOYEE
Admin trying to DELETE...
Executing command: DELETE EMPLOYEE
```

---

### ✅ **Summary**
- `READ` command → allowed for any user (Admin or User).
- `CREATE` / `DELETE` command → only allowed for **ADMIN**.
- **Proxy** checks permissions, then **delegates** to real object.


<br/>

## 🖼️ Ex-2: Combined Example: Virtual Proxy + Cache Proxy

Imagine you are loading large images from disk.  
You **don't want** to load the same image again if it was already loaded.

- **First time** → load from disk (slow)
- **Next time** → serve from cache (fast)

---
  
## 1. Image interface

```java
package LowLevelDesign.DesignPattern;

public interface Image {
    void display();
}
```

---

## 2. RealImage class (expensive to load)

```java
package LowLevelDesign.DesignPattern;

public class RealImage implements Image {
    private String filename;

    public RealImage(String filename) {
        this.filename = filename;
        loadFromDisk();
    }

    private void loadFromDisk() {
        System.out.println("Loading image from disk: " + filename);
    }

    @Override
    public void display() {
        System.out.println("Displaying: " + filename);
    }
}
```

---

## 3. ProxyImage class (Lazy Load + Cache)

```java
package LowLevelDesign.DesignPattern;

import java.util.HashMap;
import java.util.Map;

public class ProxyImage implements Image {
    private String filename;
    private static Map<String, RealImage> cache = new HashMap<>();

    public ProxyImage(String filename) {
        this.filename = filename;
    }

    @Override
    public void display() {
        RealImage realImage = cache.get(filename);

        if (realImage == null) {
            System.out.println("Image not in cache, loading...");
            realImage = new RealImage(filename);
            cache.put(filename, realImage);
        } else {
            System.out.println("Image found in cache, no need to load from disk.");
        }
        
        realImage.display();
    }
}
```

---

## 4. Main class to test

```java
package LowLevelDesign.DesignPattern;

public class ProxyPatternDemo {
    public static void main(String[] args) {
        Image img1 = new ProxyImage("test_image.jpg");
        img1.display(); // Load from disk

        System.out.println("----------------------");

        Image img2 = new ProxyImage("test_image.jpg");
        img2.display(); // Load from cache

        System.out.println("----------------------");

        Image img3 = new ProxyImage("another_image.jpg");
        img3.display(); // Load new image from disk
    }
}
```

---

## 🛠️ Output

```
Image not in cache, loading...
Loading image from disk: test_image.jpg
Displaying: test_image.jpg
----------------------
Image found in cache, no need to load from disk.
Displaying: test_image.jpg
----------------------
Image not in cache, loading...
Loading image from disk: another_image.jpg
Displaying: another_image.jpg
```

---

# 🎯 What Happened?

| Action | Behavior |
|--------|----------|
| First load of `test_image.jpg` | Loaded from disk (slow) |
| Second load of `test_image.jpg` | Loaded from cache (fast) |
| Load `another_image.jpg` | Loaded from disk |

✅ **Virtual Proxy** = **Lazy loading** (load image only when needed)  
✅ **Cache Proxy** = **Cache loaded image** (don't reload same image)
