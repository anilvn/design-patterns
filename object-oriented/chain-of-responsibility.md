
# Chain of Responsibility

The **Chain of Responsibility** pattern is a behavioral design pattern that allows request handling to be passed along a chain of handlers. Each handler decides whether to process the request or pass it to the next handler in the chain.  

## **Example: Logging System using Chain of Responsibility**  
Let's implement a logging system where different log levels (**INFO, DEBUG, ERROR**) are handled by different loggers.

---

### **Step 1: Create an Abstract Logger**
Each logger should have a reference to the next logger in the chain.

```java
abstract class Logger {
    public static int INFO = 1;
    public static int DEBUG = 2;
    public static int ERROR = 3;

    protected int level;
    protected Logger nextLogger;

    public void setNextLogger(Logger nextLogger) {
        this.nextLogger = nextLogger;
    }

    public void logMessage(int level, String message) {
        if (this.level <= level) {
            write(message);
        }
        if (nextLogger != null) {
            nextLogger.logMessage(level, message);
        }
    }

    protected abstract void write(String message);
}
```

---

### **Step 2: Implement Concrete Loggers**
Each logger handles a specific level of logging.

#### **InfoLogger**
```java
class InfoLogger extends Logger {
    public InfoLogger(int level) {
        this.level = level;
    }

    @Override
    protected void write(String message) {
        System.out.println("INFO: " + message);
    }
}
```

#### **DebugLogger**
```java
class DebugLogger extends Logger {
    public DebugLogger(int level) {
        this.level = level;
    }

    @Override
    protected void write(String message) {
        System.out.println("DEBUG: " + message);
    }
}
```

#### **ErrorLogger**
```java
class ErrorLogger extends Logger {
    public ErrorLogger(int level) {
        this.level = level;
    }

    @Override
    protected void write(String message) {
        System.out.println("ERROR: " + message);
    }
}
```

---

### **Step 3: Create the Chain of Loggers**
```java
class LoggerChain {
    public static Logger getChainOfLoggers() {
        Logger errorLogger = new ErrorLogger(Logger.ERROR);
        Logger debugLogger = new DebugLogger(Logger.DEBUG);
        Logger infoLogger = new InfoLogger(Logger.INFO);

        infoLogger.setNextLogger(debugLogger);
        debugLogger.setNextLogger(errorLogger);

        return infoLogger;  // Returning the first logger in the chain
    }
}
```

---

### **Step 4: Test the Logging Chain**
```java
public class ChainOfResponsibilityExample {
    public static void main(String[] args) {
        Logger loggerChain = LoggerChain.getChainOfLoggers();

        loggerChain.logMessage(Logger.INFO, "This is an informational message.");
        System.out.println("----------------------");
        loggerChain.logMessage(Logger.DEBUG, "This is a debug-level message.");
        System.out.println("----------------------");
        loggerChain.logMessage(Logger.ERROR, "This is an error message.");
    }
}
```

---

### **Output**
```
INFO: This is an informational message.
----------------------
DEBUG: This is a debug-level message.
ERROR: This is an error message.
----------------------
ERROR: This is an error message.
```

---

### **How It Works**
1. The `LoggerChain` creates a chain where `InfoLogger → DebugLogger → ErrorLogger`.
2. The `logMessage()` method checks if the logger can handle the given level.
3. If it can handle the message, it logs it.
4. The request is passed to the next logger in the chain until it is fully handled.

This approach makes the system flexible and extensible since we can easily add new loggers without modifying the existing code.


<br/><br/><br/><br/>

## EX:  ATM Machine using the Chain of Responsibility pattern.

Let's design an **ATM Machine** using the **Chain of Responsibility** pattern.  
The ATM should dispense money in denominations of **2000, 500, and 100** as per the requested amount.

---

## **Steps to Implement**
1. **Create an abstract handler (ATMDispenser)**  
   - Defines the template method `dispense()`  
   - Holds a reference to the next handler in the chain.

2. **Create concrete handlers (Dispenser for 2000, 500, and 100 notes)**  
   - Each handler will dispense its respective denomination first.  
   - Passes the remaining amount to the next handler in the chain.

3. **Set up the chain and test it.**

---

### **Step 1: Abstract Handler**
```java
abstract class ATMDispenser {
    protected ATMDispenser nextDispenser;

    public void setNextDispenser(ATMDispenser nextDispenser) {
        this.nextDispenser = nextDispenser;
    }

    public void dispense(int amount) {
        if (nextDispenser != null) {
            nextDispenser.dispense(amount);
        }
    }
}
```

---

### **Step 2: Concrete Handlers for Different Denominations**
Each handler will:
- Dispense as many notes as possible for its denomination.
- Pass the remaining amount to the next dispenser.

#### **2000 Rupees Dispenser**
```java
class Rupee2000Dispenser extends ATMDispenser {
    @Override
    public void dispense(int amount) {
        if (amount >= 2000) {
            int numNotes = amount / 2000;
            int remainder = amount % 2000;
            System.out.println("Dispensing " + numNotes + " x ₹2000 notes");

            if (remainder != 0 && nextDispenser != null) {
                nextDispenser.dispense(remainder);
            }
        } else {
            super.dispense(amount);
        }
    }
}
```

---

#### **500 Rupees Dispenser**
```java
class Rupee500Dispenser extends ATMDispenser {
    @Override
    public void dispense(int amount) {
        if (amount >= 500) {
            int numNotes = amount / 500;
            int remainder = amount % 500;
            System.out.println("Dispensing " + numNotes + " x ₹500 notes");

            if (remainder != 0 && nextDispenser != null) {
                nextDispenser.dispense(remainder);
            }
        } else {
            super.dispense(amount);
        }
    }
}
```

---

#### **100 Rupees Dispenser**
```java
class Rupee100Dispenser extends ATMDispenser {
    @Override
    public void dispense(int amount) {
        if (amount >= 100) {
            int numNotes = amount / 100;
            int remainder = amount % 100;
            System.out.println("Dispensing " + numNotes + " x ₹100 notes");

            if (remainder != 0) {
                System.out.println("Cannot dispense remaining ₹" + remainder);
            }
        } else {
            System.out.println("Cannot dispense ₹" + amount + ". Amount should be in multiples of 100.");
        }
    }
}
```

---

### **Step 3: Create the ATM Chain**
```java
class ATM {
    private ATMDispenser chain;

    public ATM() {
        // Create handlers
        ATMDispenser dispenser2000 = new Rupee2000Dispenser();
        ATMDispenser dispenser500 = new Rupee500Dispenser();
        ATMDispenser dispenser100 = new Rupee100Dispenser();

        // Set the chain: 2000 → 500 → 100
        dispenser2000.setNextDispenser(dispenser500);
        dispenser500.setNextDispenser(dispenser100);

        this.chain = dispenser2000;
    }

    public void withdraw(int amount) {
        if (amount % 100 != 0) {
            System.out.println("Invalid amount! Please enter multiples of 100.");
            return;
        }
        chain.dispense(amount);
    }
}
```

---

### **Step 4: Test the ATM Machine**
```java
public class ATMDispenserTest {
    public static void main(String[] args) {
        ATM atm = new ATM();

        System.out.println("Requesting ₹3800:");
        atm.withdraw(3800);

        System.out.println("\nRequesting ₹750:");
        atm.withdraw(750);

        System.out.println("\nRequesting ₹2600:");
        atm.withdraw(2600);
    }
}
```

---

### **Output**
```
Requesting ₹3800:
Dispensing 1 x ₹2000 notes
Dispensing 3 x ₹500 notes
Dispensing 3 x ₹100 notes

Requesting ₹750:
Invalid amount! Please enter multiples of 100.

Requesting ₹2600:
Dispensing 1 x ₹2000 notes
Dispensing 1 x ₹500 notes
Dispensing 1 x ₹100 notes
```

---

## **How It Works**
1. **2000 Dispenser** checks if it can dispense 2000 rupee notes.  
2. **500 Dispenser** handles the remaining amount.  
3. **100 Dispenser** handles the smallest denomination.  
4. If the amount is not a multiple of 100, the ATM rejects it.

This approach makes the ATM **scalable** and **flexible**, allowing us to easily add new denominations (e.g., ₹200 or ₹50) without modifying existing code. 🚀