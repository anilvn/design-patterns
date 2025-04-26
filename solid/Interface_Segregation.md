

## What is the Interface Segregation Principle?

The **Interface Segregation Principle (ISP)** is the fourth principle in the SOLID design principles.

* ISP states: **"A class should not be forced to implement interfaces it does not use."**
* This means that instead of creating large, monolithic interfaces, we should split them into smaller, more specific interfaces.
* Clients should only depend on the methods they actually use.

Key concepts of ISP:

* **Interface pollution**: Avoid forcing classes to implement methods they don't need
* **Role-based interfaces**: Design interfaces around specific roles or behaviors
* **Granularity**: Prefer many small, focused interfaces over large, general-purpose ones
* **Client-specific interfaces**: Tailor interfaces to client needs

Benefits of applying ISP:

* **Reduced coupling**: Classes only depend on what they actually use
* **Better maintainability**: Changes to one interface affect fewer classes
* **Improved readability**: Interfaces clearly communicate their purpose
* **Enhanced flexibility**: Classes can implement multiple small interfaces to combine behaviors

Signs that ISP is being violated:

* Interface methods that return `null` or throw `UnsupportedOperationException`
* Classes that implement methods with empty bodies
* Interfaces with many unrelated methods
* Client code that only uses a small subset of an interface's methods

ISP works closely with the Single Responsibility Principle (SRP) but focuses specifically on interface design.

[Back to Top](#table-of-contents)

## How to implement ISP in printer interfaces?

The following example demonstrates applying the Interface Segregation Principle to printer device interfaces.

### Violation of ISP:

```java
interface Printer {
    void print();
    void scan();
    void fax();
    void printDuplex();
}

class BasicPrinter implements Printer {
    @Override
    public void print() {
        System.out.println("Printing document...");
        // Implementation for printing
    }

    @Override
    public void scan() {
        // Cannot scan
        throw new UnsupportedOperationException("Scan not supported!");
    }

    @Override
    public void fax() {
        // Cannot fax
        throw new UnsupportedOperationException("Fax not supported!");
    }
    
    @Override
    public void printDuplex() {
        // Cannot print duplex
        throw new UnsupportedOperationException("Duplex printing not supported!");
    }
}
```

**Problem:**
* `BasicPrinter` only supports printing, but it is forced to implement `scan()`, `fax()`, and `printDuplex()` methods
* The class must provide implementations for methods it doesn't support
* This violates ISP because the interface is too large and includes methods not applicable to all implementations
* Clients depending on `Printer` need to be aware of which methods might throw exceptions

### ISP-Compliant Solution:

```java
// Separate interfaces for different functionalities
interface Printer {
    void print();
}

interface Scanner {
    void scan();
}

interface FaxMachine {
    void fax();
}

interface DuplexPrinter {
    void printDuplex();
}

// Basic printer only implements what it can do
class BasicPrinter implements Printer {
    @Override
    public void print() {
        System.out.println("Printing document...");
        // Implementation for printing
    }
}

// All-in-one device implements multiple interfaces
class AllInOnePrinter implements Printer, Scanner, FaxMachine {
    @Override
    public void print() {
        System.out.println("Printing document...");
        // Implementation for printing
    }

    @Override
    public void scan() {
        System.out.println("Scanning document...");
        // Implementation for scanning
    }

    @Override
    public void fax() {
        System.out.println("Sending fax...");
        // Implementation for faxing
    }
}

// Advanced printer implements printer and duplex printing
class AdvancedPrinter implements Printer, DuplexPrinter {
    @Override
    public void print() {
        System.out.println("Printing document...");
        // Implementation for printing
    }
    
    @Override
    public void printDuplex() {
        System.out.println("Printing on both sides...");
        // Implementation for duplex printing
    }
}
```

**Example Usage:**
```java
public class Main {
    public static void main(String[] args) {
        // Using basic printer
        Printer basicPrinter = new BasicPrinter();
        printDocument(basicPrinter);
        
        // Using all-in-one printer
        AllInOnePrinter multiFunction = new AllInOnePrinter();
        printDocument(multiFunction);
        scanDocument(multiFunction);
        sendFax(multiFunction);
        
        // Using advanced printer with duplex capability
        AdvancedPrinter advancedPrinter = new AdvancedPrinter();
        printDocument(advancedPrinter);
        printDuplexDocument(advancedPrinter);
        
        /* Sample output:
         * Printing document...
         * Printing document...
         * Scanning document...
         * Sending fax...
         * Printing document...
         * Printing on both sides...
         */
    }
    
    // Clients only depend on the interfaces they need
    public static void printDocument(Printer printer) {
        printer.print();
    }
    
    public static void scanDocument(Scanner scanner) {
        scanner.scan();
    }
    
    public static void sendFax(FaxMachine faxMachine) {
        faxMachine.fax();
    }
    
    public static void printDuplexDocument(DuplexPrinter duplexPrinter) {
        duplexPrinter.printDuplex();
    }
}
```

**Comparison: ISP Violation vs ISP-Compliant**

| Aspect | ISP Violation | ISP-Compliant |
|--------|---------------|---------------|
| Interface Size | One large interface | Multiple small interfaces |
| Method Implementation | Forced to implement unused methods | Only implements needed methods |
| Exception Handling | Throws exceptions for unsupported operations | No need for exceptions |
| Client Coupling | High coupling to irrelevant methods | Coupling only to needed methods |
| Flexibility | Limited | High (mix and match interfaces) |

**Key Improvements:**
* Each device implements only the interfaces it supports
* No methods throw `UnsupportedOperationException`
* Client code can work with more specific interfaces
* New functionality can be added via new interfaces without affecting existing code
* Each interface represents a specific role or capability

[Back to Top](#table-of-contents)
