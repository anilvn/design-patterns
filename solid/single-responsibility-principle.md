
## What is the Single Responsibility Principle?

The **Single Responsibility Principle (SRP)** is one of the five SOLID principles of object-oriented programming and design.

* SRP states: **"A class should have only one reason to change."**
* This means each class should focus on a **single functionality** and should not be responsible for multiple concerns.
* Following this principle helps create more maintainable, scalable, and testable code.

Key benefits of applying SRP:

* **Improved maintainability**: Changes to one aspect of your application won't affect unrelated parts
* **Enhanced code organization**: Each class has a clear, well-defined purpose
* **Better testability**: Classes with single responsibilities are easier to test
* **Reduced coupling**: Fewer dependencies between components

When a class violates SRP, it typically means it's doing too much and should be broken down into smaller, more focused classes.

[Back to Top](#table-of-contents)

## How to implement SRP with file operations?

The following example demonstrates how to apply the Single Responsibility Principle to file operations by separating responsibilities into different classes.

### Violation of SRP:

```java
class FileManager {
    public void readFile(String filePath) {
        System.out.println("Reading file: " + filePath);
        // Implementation for reading file
    }

    public void writeFile(String filePath, String data) {
        System.out.println("Writing data to file: " + filePath);
        // Implementation for writing to file
    }

    public void logError(String message) {
        System.out.println("Logging error: " + message);
        // Implementation for logging errors
    }
}
```

**Problem:**
* `FileManager` is handling both file operations and logging errors
* If we change the logging mechanism, we will have to modify `FileManager`, violating SRP
* The class has multiple reasons to change

### SRP-Compliant Solution:

```java
class FileManager {
    public void readFile(String filePath) {
        System.out.println("Reading file: " + filePath);
        // Implementation for reading file
    }

    public void writeFile(String filePath, String data) {
        System.out.println("Writing data to file: " + filePath);
        // Implementation for writing to file
    }
}

// Separate class for logging
class Logger {
    public void logError(String message) {
        System.out.println("Logging error: " + message);
        // Implementation for logging errors
    }
}
```

**Example Usage:**
```java
public class Main {
    public static void main(String[] args) {
        // Using the file manager
        FileManager fileManager = new FileManager();
        fileManager.readFile("data.txt");
        fileManager.writeFile("output.txt", "Hello World");
        
        // Using the logger
        Logger logger = new Logger();
        logger.logError("File not found");
        
        /* Sample output:
         * Reading file: data.txt
         * Writing data to file: output.txt
         * Logging error: File not found
         */
    }
}
```

**Key Improvements:**
* `FileManager` now only handles file operations
* `Logger` is responsible solely for logging
* Each class has only one reason to change
* The responsibilities are properly separated

[Back to Top](#table-of-contents)

## How to implement SRP in order processing?

This example demonstrates how to apply the Single Responsibility Principle to an order processing system.

### Violation of SRP:

```java
class Order {
    public void calculateTotal() {
        System.out.println("Calculating order total...");
        // Implementation for calculating order total
    }

    public void saveToDatabase() {
        System.out.println("Saving order to database...");
        // Implementation for database operations
    }

    public void sendEmailConfirmation() {
        System.out.println("Sending order confirmation email...");
        // Implementation for email sending
    }
}
```

**Problem:**
* The `Order` class is responsible for:
  * Business logic (calculating total)
  * Data persistence (saving to database)
  * Communication (sending emails)
* Each of these responsibilities could change for different reasons
* Changes to email functionality would require modifying the `Order` class

### SRP-Compliant Solution:

```java
class Order {
    // Order details as fields
    private String orderId;
    private List<Item> items;
    
    public void calculateTotal() {
        System.out.println("Calculating order total...");
        // Implementation for calculating order total
    }
}

class OrderRepository {
    public void saveToDatabase(Order order) {
        System.out.println("Saving order to database...");
        // Implementation for database operations
    }
}

class EmailService {
    public void sendEmailConfirmation(Order order) {
        System.out.println("Sending order confirmation email...");
        // Implementation for email sending
    }
}
```

**Example Usage:**
```java
public class Main {
    public static void main(String[] args) {
        // Create an order and calculate total
        Order order = new Order();
        order.calculateTotal();
        
        // Save the order to database
        OrderRepository repository = new OrderRepository();
        repository.saveToDatabase(order);
        
        // Send confirmation email
        EmailService emailService = new EmailService();
        emailService.sendEmailConfirmation(order);
        
        /* Sample output:
         * Calculating order total...
         * Saving order to database...
         * Sending order confirmation email...
         */
    }
}
```

**Key Improvements:**
* Each class has a single, well-defined responsibility:
  * `Order` - Business logic for orders
  * `OrderRepository` - Database operations
  * `EmailService` - Email notifications
* Changes to email functionality only affect the `EmailService` class
* Changes to database operations only affect the `OrderRepository` class
* Each class has only one reason to change

[Back to Top](#table-of-contents)