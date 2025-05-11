
# Null Object Pattern

The **Null Object Pattern** is a design pattern used in Java to handle `null` values gracefully by providing a default implementation instead of returning `null`. This prevents `NullPointerException` and simplifies code by eliminating the need for explicit `null` checks.

---

### **Implementation of Null Object Pattern in Java**

Let's take an example where we have a `Customer` class, and instead of returning `null`, we return a `NullCustomer` object.

#### **Step 1: Create an Abstract Class or Interface**
```java
public abstract class Customer {
    protected String name;

    public abstract String getName();
    public abstract boolean isNull();
}
```

#### **Step 2: Create a Real Customer Class**
```java
public class RealCustomer extends Customer {
    public RealCustomer(String name) {
        this.name = name;
    }

    @Override
    public String getName() {
        return name;
    }

    @Override
    public boolean isNull() {
        return false;
    }
}
```

#### **Step 3: Create a Null Object Class**
```java
public class NullCustomer extends Customer {
    @Override
    public String getName() {
        return "Not Available";
    }

    @Override
    public boolean isNull() {
        return true;
    }
}
```

#### **Step 4: Factory or Service Class to Return Real or Null Object**
```java
public class CustomerFactory {
    private static final String[] existingCustomers = {"Anil", "John", "Alice"};

    public static Customer getCustomer(String name) {
        for (String existingCustomer : existingCustomers) {
            if (existingCustomer.equalsIgnoreCase(name)) {
                return new RealCustomer(name);
            }
        }
        return new NullCustomer();
    }
}
```

#### **Step 5: Client Code**
```java
public class NullObjectPatternDemo {
    public static void main(String[] args) {
        Customer customer1 = CustomerFactory.getCustomer("Anil");
        Customer customer2 = CustomerFactory.getCustomer("David"); // Doesn't exist
        
        System.out.println("Customer 1: " + customer1.getName());
        System.out.println("Customer 2: " + customer2.getName()); // Returns "Not Available"
        
        if (!customer2.isNull()) {
            System.out.println("Processing customer: " + customer2.getName());
        } else {
            System.out.println("Customer not found.");
        }
    }
}
```

---

### **Benefits of the Null Object Pattern**
1. **Avoids `NullPointerException`** – No need to check for `null` before calling methods.
2. **Simplifies Code** – Reduces the need for `if-else` checks for `null`.
3. **Provides Default Behavior** – Offers a fallback when real data is missing.

This pattern is particularly useful in **database queries, collections, and service responses** where a `null` result is possible.