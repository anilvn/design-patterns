
## What is the Open/Closed Principle?

The **Open/Closed Principle (OCP)** is the second principle in the SOLID design principles.

* OCP states: **"A class should be open for extension, but closed for modification."**
* This means you should be able to extend a class's behavior without modifying its source code.
* It helps in writing scalable and maintainable code by using abstraction, interfaces, and polymorphism.

Key concepts of OCP:

* **Open for extension**: You can add new functionality by creating new classes that implement or extend existing ones
* **Closed for modification**: Existing code remains unchanged, reducing the risk of bugs
* **Use of abstraction**: Interfaces and abstract classes help achieve OCP
* **Polymorphism**: Different implementations can be used interchangeably

Benefits of applying OCP:

* **Reduced risk**: Existing, tested code isn't modified
* **Improved maintainability**: New features don't require changes to existing code
* **Better scalability**: The system can grow with new functionality
* **Lower regression risk**: Less chance of breaking existing features

OCP is especially useful when designing frameworks and libraries that will be used by other developers.

[Back to Top](#table-of-contents)

## How to implement OCP in payment processing?

This example shows how to implement the Open/Closed Principle in a payment processing system.

### Violation of OCP:

```java
class PaymentProcessor {
    public void processPayment(String paymentType) {
        if (paymentType.equals("CreditCard")) {
            System.out.println("Processing credit card payment...");
            // Credit card payment logic
        } else if (paymentType.equals("PayPal")) {
            System.out.println("Processing PayPal payment...");
            // PayPal payment logic
        } else {
            System.out.println("Invalid payment method.");
        }
    }
}
```

**Problem:**
* To add new payment methods (like UPI, Google Pay, Apple Pay), we must modify the `processPayment()` method
* Each new payment type requires changes to existing code
* The class is not closed for modification

### OCP-Compliant Solution:

```java
// Step 1: Create an interface for payments
interface PaymentMethod {
    void processPayment();
}

// Step 2: Implement different payment methods
class CreditCardPayment implements PaymentMethod {
    public void processPayment() {
        System.out.println("Processing credit card payment...");
        // Credit card payment logic
    }
}

class PayPalPayment implements PaymentMethod {
    public void processPayment() {
        System.out.println("Processing PayPal payment...");
        // PayPal payment logic
    }
}

// Step 3: Modify PaymentProcessor to work with any payment method
class PaymentProcessor {
    public void processPayment(PaymentMethod paymentMethod) {
        paymentMethod.processPayment();
    }
}
```

**Example Usage:**
```java
public class Main {
    public static void main(String[] args) {
        PaymentProcessor processor = new PaymentProcessor();
        
        // Process credit card payment
        PaymentMethod creditCard = new CreditCardPayment();
        processor.processPayment(creditCard);

        // Process PayPal payment
        PaymentMethod paypal = new PayPalPayment();
        processor.processPayment(paypal);
        
        // Adding a new payment method (Google Pay)
        PaymentMethod googlePay = new GooglePayPayment();
        processor.processPayment(googlePay);
        
        /* Sample output:
         * Processing credit card payment...
         * Processing PayPal payment...
         * Processing Google Pay payment...
         */
    }
}

// New payment method added WITHOUT modifying existing code
class GooglePayPayment implements PaymentMethod {
    public void processPayment() {
        System.out.println("Processing Google Pay payment...");
        // Google Pay payment logic
    }
}
```

**Key Improvements:**
* `PaymentProcessor` is now **closed for modification**
* The system is **open for extension** - new payment methods can be added without changing existing code
* Adding new payment types only requires creating new classes that implement the `PaymentMethod` interface
* Existing payment processing code remains untouched when adding new functionality

[Back to Top](#table-of-contents)

## How to implement OCP in notification systems?

The following example demonstrates implementing the Open/Closed Principle in a notification system.

### Violation of OCP:

```java
class NotificationService {
    public void sendNotification(String type) {
        if (type.equals("Email")) {
            System.out.println("Sending Email Notification...");
            // Email notification logic
        } else if (type.equals("SMS")) {
            System.out.println("Sending SMS Notification...");
            // SMS notification logic
        } else {
            System.out.println("Invalid Notification Type");
        }
    }
}
```

**Problem:**
* Adding new notification types (e.g., WhatsApp, Push Notification) requires modifying the `sendNotification()` method
* Every new notification channel forces changes to existing code
* The class is not closed for modification

### OCP-Compliant Solution:

```java
// Step 1: Create an interface for notifications
interface Notification {
    void send();
}

// Step 2: Implement different notification types
class EmailNotification implements Notification {
    public void send() {
        System.out.println("Sending Email Notification...");
        // Email notification logic
    }
}

class SMSNotification implements Notification {
    public void send() {
        System.out.println("Sending SMS Notification...");
        // SMS notification logic
    }
}

// Step 3: Modify NotificationService to work with any notification type
class NotificationService {
    public void sendNotification(Notification notification) {
        notification.send();
    }
}
```

**Example Usage:**
```java
public class Main {
    public static void main(String[] args) {
        NotificationService service = new NotificationService();

        // Send email notification
        Notification email = new EmailNotification();
        service.sendNotification(email);

        // Send SMS notification
        Notification sms = new SMSNotification();
        service.sendNotification(sms);
        
        // Adding a new notification type (WhatsApp)
        Notification whatsapp = new WhatsAppNotification();
        service.sendNotification(whatsapp);
        
        /* Sample output:
         * Sending Email Notification...
         * Sending SMS Notification...
         * Sending WhatsApp Notification...
         */
    }
}

// New notification type added WITHOUT modifying existing code
class WhatsAppNotification implements Notification {
    public void send() {
        System.out.println("Sending WhatsApp Notification...");
        // WhatsApp notification logic
    }
}
```

**Comparison: OCP Violation vs OCP-Compliant**

| Aspect | OCP Violation | OCP-Compliant |
|--------|---------------|---------------|
| Adding new types | Requires modifying existing class | Only requires adding new classes |
| Risk of bugs | High (changing working code) | Low (existing code untouched) |
| Maintainability | Poor (growing if-else chains) | Good (modular design) |
| Testability | Hard (logic mixed) | Easy (separate classes) |
| Scalability | Limited | Excellent |

**Key Improvements:**
* `NotificationService` is now **closed for modification**
* The system is **open for extension** - new notification types can be added without changing existing code
* Adding new notification channels only requires creating new classes that implement the `Notification` interface
* The design is more maintainable and scalable

[Back to Top](#table-of-contents)