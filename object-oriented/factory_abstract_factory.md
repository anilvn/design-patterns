# Factory vs Abstract Factory Patterns

## Factory Pattern
A Factory Pattern is a creational design pattern that provides an interface for creating objects without specifying their concrete classes. The client code requests an object, and the factory determines which specific implementation to instantiate.

Key characteristics:
- Creates objects of a single type (with potentially different subtypes)
- Usually implemented as a single method that returns an interface or abstract class
- Client knows which factory to use but doesn't know the specific implementation it will receive

## Abstract Factory Pattern
An Abstract Factory Pattern is a creational design pattern that provides an interface for creating families of related or dependent objects without specifying their concrete classes. It's essentially a "factory of factories."

Key characteristics:
- Creates families of related objects
- Involves multiple factory methods, each creating a different kind of object
- Ensures that all created objects work together coherently
- Client works with factories and products through abstract interfaces

The main difference is that a Factory creates objects of a single type, while an Abstract Factory creates families of related objects that are designed to work together.

## 📋 Comparison Between Factory Method and Abstract Factory Patterns

| Factory Method Pattern | Abstract Factory Pattern |
|:------------------------|:--------------------------|
| Creates objects using a single factory method. | Creates families of related objects. |
| Typically used to instantiate one type of object. | Supports multiple types of related objects (e.g., Buttons and TextFields, or Banks and Loans). |
| Focuses on **one product** at a time. | Focuses on **multiple products** that are designed to work together. |
| Client is aware of the specific factory it is using. | Client only interacts with the abstract factory; doesn't know about concrete factories. |
| Easier to implement and maintain. | More complex because it deals with multiple factories and products. |
| Example: Creating a `Car` object depending on the type (e.g., Sedan, SUV). | Example: Creating a family of UI components (e.g., LightButton, LightTextField or DarkButton, DarkTextField) based on a theme. |

---

### Example: Creating Different Types of Notification Services using the Factory Pattern

```java
// Step 1: Define an interface for Notification
interface Notification {
    void notifyUser();
}

// Step 2: Implement different types of notifications
class EmailNotification implements Notification {
    public void notifyUser() {
        System.out.println("Sending an Email Notification...");
    }
}

class SMSNotification implements Notification {
    public void notifyUser() {
        System.out.println("Sending an SMS Notification...");
    }
}

class PushNotification implements Notification {
    public void notifyUser() {
        System.out.println("Sending a Push Notification...");
    }
}

// Step 3: Create a Factory Class
class NotificationFactory {
    public static Notification createNotification(String type) {
        if (type.equalsIgnoreCase("EMAIL")) {
            return new EmailNotification();
        } else if (type.equalsIgnoreCase("SMS")) {
            return new SMSNotification();
        } else if (type.equalsIgnoreCase("PUSH")) {
            return new PushNotification();
        }
        return null;
    }
}

// Step 4: Use the Factory
public class FactoryPatternExample {
    public static void main(String[] args) {
        Notification notification = NotificationFactory.createNotification("EMAIL");
        notification.notifyUser();
    }
}

```

### output
```yaml
Sending an Email Notification...

```


### Example: Creating Different Payment Methods for Different Banks using Abstract Factory Pattern

### **Example: UI Widget Toolkit (Light Theme / Dark Theme)**

Imagine you're building a UI library where depending on the theme (Light or Dark), you need to create **Buttons** and **TextFields** that look different.

✅ Light Theme → LightButton, LightTextField  
✅ Dark Theme → DarkButton, DarkTextField

---

#### **Step 1: Create Product Interfaces**
```java
// Common interfaces for Buttons and TextFields
interface Button {
    void paint();
}

interface TextField {
    void render();
}
```

---

#### **Step 2: Create Concrete Product Variants**
```java
// Light Theme Implementations
class LightButton implements Button {
    public void paint() {
        System.out.println("Rendering a Light Theme Button");
    }
}

class LightTextField implements TextField {
    public void render() {
        System.out.println("Rendering a Light Theme Text Field");
    }
}

// Dark Theme Implementations
class DarkButton implements Button {
    public void paint() {
        System.out.println("Rendering a Dark Theme Button");
    }
}

class DarkTextField implements TextField {
    public void render() {
        System.out.println("Rendering a Dark Theme Text Field");
    }
}
```

---

#### **Step 3: Create Abstract Factory**
```java
interface UIFactory {
    Button createButton();
    TextField createTextField();
}
```

---

#### **Step 4: Create Concrete Factories**
```java
// Light Theme Factory
class LightThemeFactory implements UIFactory {
    public Button createButton() {
        return new LightButton();
    }

    public TextField createTextField() {
        return new LightTextField();
    }
}

// Dark Theme Factory
class DarkThemeFactory implements UIFactory {
    public Button createButton() {
        return new DarkButton();
    }

    public TextField createTextField() {
        return new DarkTextField();
    }
}
```

---

#### **Step 5: Factory Producer**
```java
class UIFactoryProducer {
    public static UIFactory getFactory(String theme) {
        if (theme.equalsIgnoreCase("LIGHT")) {
            return new LightThemeFactory();
        } else if (theme.equalsIgnoreCase("DARK")) {
            return new DarkThemeFactory();
        }
        return null;
    }
}
```

---

#### **Step 6: Client Code (Usage)**
```java
public class AbstractFactoryUIExample {
    public static void main(String[] args) {
        // Suppose the theme is DARK
        UIFactory uiFactory = UIFactoryProducer.getFactory("DARK");

        Button button = uiFactory.createButton();
        TextField textField = uiFactory.createTextField();

        button.paint();
        textField.render();
    }
}
```

---

#### **Output:**
```
Rendering a Dark Theme Button
Rendering a Dark Theme Text Field
```

---

### 🧠 **Why is this a good real-world example?**
- UI frameworks (like Android, iOS, Web apps) often need to **generate multiple families of related components**.
- **Theme switching** is a classic Abstract Factory use case.
- Helps in keeping the code **extensible and maintainable**.

---
