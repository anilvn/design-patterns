# Facade Pattern


> **Facade Design Pattern** is a structural design pattern that provides a simplified, unified interface to a complex subsystem or set of interfaces. It hides the complexities of the system by encapsulating the interactions between multiple components behind a single high-level interface, making the system easier to use and maintain.

### **Scenario:**  
We have a **Banking System** where a user can perform operations like withdrawing cash, checking balance, and depositing money. Instead of interacting with multiple subsystems directly, we provide a unified interface using a **BankFacade**.

---

### **Step 1: Subsystem Classes**
```java
// Subsystem 1: Account Number Check
class AccountNumberCheck {
    private int accountNumber = 12345678;

    public boolean isAccountValid(int accNumber) {
        return accNumber == accountNumber;
    }
}

// Subsystem 2: Security Code Check
class SecurityCodeCheck {
    private int securityCode = 1234;

    public boolean isCodeValid(int secCode) {
        return secCode == securityCode;
    }
}

// Subsystem 3: Funds Check
class FundsCheck {
    private double balance = 1000.0;

    public double getBalance() {
        return balance;
    }

    public boolean withdraw(double amount) {
        if (amount > balance) {
            System.out.println("Insufficient Funds!");
            return false;
        }
        balance -= amount;
        System.out.println("Withdrawal Successful! New Balance: " + balance);
        return true;
    }

    public void deposit(double amount) {
        balance += amount;
        System.out.println("Deposit Successful! New Balance: " + balance);
    }
}
```

---

### **Step 2: Facade Class**
```java
// Facade Class: Simplifies access to the banking system
class BankFacade {
    private AccountNumberCheck accountChecker;
    private SecurityCodeCheck securityChecker;
    private FundsCheck fundsChecker;

    public BankFacade(int accNumber, int secCode) {
        accountChecker = new AccountNumberCheck();
        securityChecker = new SecurityCodeCheck();
        fundsChecker = new FundsCheck();

        if (!accountChecker.isAccountValid(accNumber) || !securityChecker.isCodeValid(secCode)) {
            throw new SecurityException("Invalid Account or Security Code!");
        }
    }

    public void withdrawCash(double amount) {
        if (fundsChecker.withdraw(amount)) {
            System.out.println("Transaction Completed.");
        }
    }

    public void depositCash(double amount) {
        fundsChecker.deposit(amount);
        System.out.println("Transaction Completed.");
    }

    public void checkBalance() {
        System.out.println("Current Balance: " + fundsChecker.getBalance());
    }
}
```

---

### **Step 3: Client Code**
```java
public class FacadePatternDemo {
    public static void main(String[] args) {
        try {
            BankFacade bankFacade = new BankFacade(12345678, 1234);

            bankFacade.checkBalance();
            bankFacade.withdrawCash(500);
            bankFacade.depositCash(300);
            bankFacade.checkBalance();
        } catch (SecurityException e) {
            System.out.println("Access Denied: " + e.getMessage());
        }
    }
}
```

---

### **Output:**
```
Current Balance: 1000.0
Withdrawal Successful! New Balance: 500.0
Transaction Completed.
Deposit Successful! New Balance: 800.0
Transaction Completed.
Current Balance: 800.0
```

---

### **Explanation:**
- **Subsystems (AccountNumberCheck, SecurityCodeCheck, FundsCheck)** handle specific tasks.
- **BankFacade** provides a simple interface for clients to interact with the system.
- The **Client (FacadePatternDemo)** interacts only with `BankFacade`, making the process seamless.

