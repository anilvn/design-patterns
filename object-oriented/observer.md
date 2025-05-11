# Observer Pattern

### ✅ **Observer Design Pattern – Definition**

> **The Observer Design Pattern** is a behavioral design pattern in which an object, called the **Subject** (or **Observable**), maintains a list of its dependents, called **Observers**, and automatically notifies them of any state changes, usually by calling one of their methods.

---

### 🔍 **Key Concepts**

- **Subject (Observable)**: The object that holds the state and notifies observers when the state changes.
- **Observers**: Objects that want to be informed about changes in the subject's state.
- **Loose Coupling**: Observers and the subject are loosely coupled; the subject doesn’t need to know the details of observers.

---

### 📦 **Real-World Examples**
- Email/SMS notifications when a product is back in stock (like your example).
- News feed updates in social media platforms.
- GUI event listeners (e.g., button click listeners in Java Swing or JavaFX).



---

### 🔄 **Folder Structure (for reference)**:
```
ObserverPattern/
├── Observable/
│   ├── IphoneObservableImpl.java
│   └── StocksObservable.java
├── Observer/
│   ├── EmailAlertObserverImpl.java
│   ├── MobileAlertObserverImpl.java
│   └── NotificationAlertObserver.java
└── Store.java
```

---

### 📄 **1. Subject (Observable)**

#### **StocksObservable.java**
```java
package ObserverPattern.Observable;

import ObserverPattern.Observer.NotificationAlertObserver;

public interface StocksObservable {
    void addObserver(NotificationAlertObserver observer);
    void removeObserver(NotificationAlertObserver observer);
    void notifyObservers();

    void setStockCount(int newStockCount);
    int getStockCount();
}
```

#### **IphoneObservableImpl.java**
```java
package ObserverPattern.Observable;

import ObserverPattern.Observer.NotificationAlertObserver;
import java.util.ArrayList;
import java.util.List;

public class IphoneObservableImpl implements StocksObservable {

    private List<NotificationAlertObserver> observers = new ArrayList<>();
    private int stockCount = 0;

    @Override
    public void addObserver(NotificationAlertObserver observer) {
        observers.add(observer);
    }

    @Override
    public void removeObserver(NotificationAlertObserver observer) {
        observers.remove(observer);
    }

    @Override
    public void notifyObservers() {
        for (NotificationAlertObserver observer : observers) {
            observer.update();
        }
    }

    @Override
    public void setStockCount(int newStockCount) {
        if (stockCount == 0 && newStockCount > 0) {
            notifyObservers(); // Notify only when stock was 0 and new stock is added
        }
        stockCount = newStockCount;
    }

    @Override
    public int getStockCount() {
        return stockCount;
    }
}
```

---

### 📄 **2. Observer Interface**

#### **NotificationAlertObserver.java**
```java
package ObserverPattern.Observer;

public interface NotificationAlertObserver {
    void update();
}
```

---

### 📄 **3. Concrete Observers**

#### **EmailAlertObserverImpl.java**
```java
package ObserverPattern.Observer;

import ObserverPattern.Observable.StocksObservable;

public class EmailAlertObserverImpl implements NotificationAlertObserver {

    private String emailId;
    private StocksObservable observable;

    public EmailAlertObserverImpl(String emailId, StocksObservable observable) {
        this.emailId = emailId;
        this.observable = observable;
    }

    @Override
    public void update() {
        sendEmail(emailId, "Product is back in stock! Hurry up and buy now!");
    }

    private void sendEmail(String emailId, String message) {
        System.out.println("Email sent to: " + emailId);
        System.out.println("Message: " + message);
    }
}
```

#### **MobileAlertObserverImpl.java**
```java
package ObserverPattern.Observer;

import ObserverPattern.Observable.StocksObservable;

public class MobileAlertObserverImpl implements NotificationAlertObserver {

    private String userName;
    private StocksObservable observable;

    public MobileAlertObserverImpl(String userName, StocksObservable observable) {
        this.userName = userName;
        this.observable = observable;
    }

    @Override
    public void update() {
        sendMobileNotification(userName, "Product is back in stock! Grab it now!");
    }

    private void sendMobileNotification(String userName, String message) {
        System.out.println("Mobile notification sent to: " + userName);
        System.out.println("Message: " + message);
    }
}
```

---

### 📄 **4. Client Code (Driver Class)**

#### **Store.java**
```java
import ObserverPattern.Observable.IphoneObservableImpl;
import ObserverPattern.Observable.StocksObservable;
import ObserverPattern.Observer.EmailAlertObserverImpl;
import ObserverPattern.Observer.MobileAlertObserverImpl;
import ObserverPattern.Observer.NotificationAlertObserver;

public class Store {
    public static void main(String[] args) {
        // Create Observable (Iphone stock)
        StocksObservable iphoneStockObservable = new IphoneObservableImpl();

        // Create Observers (Email and Mobile)
        NotificationAlertObserver emailObserver1 = new EmailAlertObserverImpl("xyz1@gmail.com", iphoneStockObservable);
        NotificationAlertObserver emailObserver2 = new EmailAlertObserverImpl("xyz2@gmail.com", iphoneStockObservable);
        NotificationAlertObserver mobileObserver = new MobileAlertObserverImpl("user123", iphoneStockObservable);

        // Register Observers
        iphoneStockObservable.addObserver(emailObserver1);
        iphoneStockObservable.addObserver(emailObserver2);
        iphoneStockObservable.addObserver(mobileObserver);

        // Set stock count (will trigger notifications if previous stock was 0)
        iphoneStockObservable.setStockCount(10);
    }
}
```

---

### 📊 **How It Works**

1. `IphoneObservableImpl` manages stock updates and notifies observers when stock becomes available.
2. `EmailAlertObserverImpl` and `MobileAlertObserverImpl` receive updates and send notifications.
3. `Store.java` ties everything together by:
   - Creating the observable (iPhone stock).
   - Registering observers (emails & mobile users).
   - Updating stock count, which triggers notifications.

---

### 🔑 **Key Improvements**
- Proper naming conventions for clarity.
- Consistent formatting for readability.
- Comments to explain important logic.
- Better control of stock updates to prevent unnecessary notifications.

---
