# Builder Design Pattern

### **Builder Design Pattern Explanation**
- The **Builder Pattern** is used to construct complex objects step by step.
- It provides a way to create an immutable object with optional parameters.
- The **main advantage** is improved code readability and flexibility.
- When you want to avoid **constructor telescoping** (multiple constructors with different combinations of parameters).
  - you can implement Builder pattern in mutalbe and immutable ways. but stick to immutable way.

---

### **Key Features of This Implementation**
1. **Encapsulation**: The `Employee` class has private fields, and values are set using the builder.
2. **Immutability**: The created `Employee` object is immutable.
3. **Fluent Interface**: Methods return `this` to allow method chaining.
4. **Optional Parameters**: You can set only the necessary fields.

---
### Example for Builder Design Pattern
```java
public class User {
    // All final fields (immutable)
    private final String firstName;     // Required
    private final String lastName;      // Required
    private final int age;              // Optional
    private final String phone;         // Optional
    private final String address;       // Optional
    private final String email;         // Optional
    
    // Private constructor - only the Builder can create User objects
    private User(UserBuilder builder) {
        this.firstName = builder.firstName;
        this.lastName = builder.lastName;
        this.age = builder.age;
        this.phone = builder.phone;
        this.address = builder.address;
        this.email = builder.email;
    }
    
    // Getters (no setters to ensure immutability)
    // for all the fiels...
    
    @Override
    public String toString() {
        return "User: " + firstName + " " + lastName + 
               ", Age: " + age + 
               ", Phone: " + phone + 
               ", Address: " + address + 
               ", Email: " + email;
    }
    
    // Builder class
    public static class UserBuilder {
        // Required parameters
        private final String firstName;
        private final String lastName;
        
        // Optional parameters - initialized with default values
        private int age = 0;
        private String phone = "";
        private String address = "";
        private String email = "";
        
        // Constructor with required parameters
        public UserBuilder(String firstName, String lastName) {
            this.firstName = firstName;
            this.lastName = lastName;
        }
        
        // Methods to set optional parameters (fluent interface)
        public UserBuilder age(int age) {
            this.age = age;
            return this;
        }
        
        public UserBuilder phone(String phone) {
            this.phone = phone;
            return this;
        }
        
        public UserBuilder address(String address) {
            this.address = address;
            return this;
        }
        
        public UserBuilder email(String email) {
            this.email = email;
            return this;
        }
        
        // Build method to create the User object
        public User build() {
            // Custom validation logic can be added here
            if (age < 0) {
                throw new IllegalArgumentException("Age cannot be negative");
            }
            return new User(this);
        }
    }
}
```

Usage example:
```java
public class BuilderDemo {
    public static void main(String[] args) {
        // Create a user with all attributes
        User user1 = new User.UserBuilder("John", "Doe")
                .age(30)
                .phone("1234567890")
                .address("123 Main St")
                .email("john.doe@example.com")
                .build();
                
        System.out.println(user1);
        
        // Create a user with only required attributes
        User user2 = new User.UserBuilder("Jane", "Smith")
                .build();
                
        System.out.println(user2);
        
        // Create a user with some attributes
        User user3 = new User.UserBuilder("Bob", "Johnson")
                .age(25)
                .email("bob@example.com")
                .build();
                
        System.out.println(user3);
        
        // Output:
        // User: John Doe, Age: 30, Phone: 1234567890, Address: 123 Main St, Email: john.doe@example.com
        // User: Jane Smith, Age: 0, Phone: , Address: , Email: 
        // User: Bob Johnson, Age: 25, Phone: , Address: , Email: bob@example.com
    }
}
```

---


### Classic Builder with Director

This implementation separates the builder interface from concrete builders and uses a director to manage the building process.

```java
// Product
class Computer {
    private String cpu;
    private String ram;
    private String storage;
    private String gpu;
    private String os;
    
    // Setters
    public void setCpu(String cpu) { this.cpu = cpu; }
    public void setRam(String ram) { this.ram = ram; }
    public void setStorage(String storage) { this.storage = storage; }
    public void setGpu(String gpu) { this.gpu = gpu; }
    public void setOs(String os) { this.os = os; }
    
    @Override
    public String toString() {
        return "Computer [CPU: " + cpu + ", RAM: " + ram + 
               ", Storage: " + storage + ", GPU: " + gpu + 
               ", OS: " + os + "]";
    }
}

// Builder interface
interface ComputerBuilder {
    void buildCpu(String cpu);
    void buildRam(String ram);
    void buildStorage(String storage);
    void buildGpu(String gpu);
    void buildOs(String os);
    Computer getComputer();
}

// Concrete builder
class DesktopBuilder implements ComputerBuilder {
    private Computer computer = new Computer();
    
    @Override
    public void buildCpu(String cpu) {
        computer.setCpu(cpu);
    }
    
    @Override
    public void buildRam(String ram) {
        computer.setRam(ram);
    }
    
    @Override
    public void buildStorage(String storage) {
        computer.setStorage(storage);
    }
    
    @Override
    public void buildGpu(String gpu) {
        computer.setGpu(gpu);
    }
    
    @Override
    public void buildOs(String os) {
        computer.setOs(os);
    }
    
    @Override
    public Computer getComputer() {
        return computer;
    }
}

// Director
class ComputerDirector {
    public Computer buildGamingComputer(ComputerBuilder builder) {
        builder.buildCpu("Intel Core i9");
        builder.buildRam("32GB DDR4");
        builder.buildStorage("2TB SSD");
        builder.buildGpu("NVIDIA RTX 3080");
        builder.buildOs("Windows 11");
        return builder.getComputer();
    }
    
    public Computer buildOfficeComputer(ComputerBuilder builder) {
        builder.buildCpu("Intel Core i5");
        builder.buildRam("16GB DDR4");
        builder.buildStorage("512GB SSD");
        builder.buildGpu("Integrated Graphics");
        builder.buildOs("Windows 10");
        return builder.getComputer();
    }
}
```

Usage of classic builder:
```java
public class ClassicBuilderDemo {
    public static void main(String[] args) {
        ComputerDirector director = new ComputerDirector();
        ComputerBuilder builder = new DesktopBuilder();
        
        // Build a gaming computer
        Computer gamingPC = director.buildGamingComputer(builder);
        System.out.println("Gaming PC: " + gamingPC);
        
        // Reset builder and build an office computer
        builder = new DesktopBuilder();
        Computer officePC = director.buildOfficeComputer(builder);
        System.out.println("Office PC: " + officePC);
        
        // Output:
        // Gaming PC: Computer [CPU: Intel Core i9, RAM: 32GB DDR4, Storage: 2TB SSD, GPU: NVIDIA RTX 3080, OS: Windows 11]
        // Office PC: Computer [CPU: Intel Core i5, RAM: 16GB DDR4, Storage: 512GB SSD, GPU: Integrated Graphics, OS: Windows 10]
    }
}
```


### StringBuilder Example (Java's Built-in Builder)

Java's `StringBuilder` is a real-world example of the Builder pattern:

```java
public class StringBuilderExample {
    public static void main(String[] args) {
        // Using Java's StringBuilder (a built-in implementation of Builder pattern)
        StringBuilder builder = new StringBuilder();
        
        // Build a string step by step
        builder.append("Hello")
               .append(" ")
               .append("World")
               .append("!")
               .append(" ")
               .append("This")
               .append(" ")
               .append("is")
               .append(" ")
               .append("StringBuilder");
        
        // Get the final product
        String result = builder.toString();
        System.out.println(result);
        
        // Output:
        // Hello World! This is StringBuilder
    }
}
```

### Lombok @Builder Example (Modern Java Development)

In modern Java development, Lombok's `@Builder` annotation can automatically generate builder code:

```java
import lombok.Builder;
import lombok.ToString;

@Builder
@ToString
public class Person {
    private String firstName;
    private String lastName;
    private int age;
    private String address;
    private String phoneNumber;
}
```

Usage with Lombok:
```java
public class LombokBuilderDemo {
    public static void main(String[] args) {
        Person person = Person.builder()
                .firstName("John")
                .lastName("Doe")
                .age(30)
                .address("123 Main St")
                .phoneNumber("555-1234")
                .build();
                
        System.out.println(person);
        
        // Output with Lombok:
        // Person(firstName=John, lastName=Doe, age=30, address=123 Main St, phoneNumber=555-1234)
    }
}
```

[Back to Top](#table-of-contents)

## What are the advantages and disadvantages of Builder Pattern?

### Advantages

- **Parameter Control**: Manages many optional parameters clearly
- **Step-by-Step Construction**: Builds complex objects incrementally
- **Immutability Support**: Creates immutable objects without telescoping constructors
- **Readable Code**: Makes client code more readable with method chaining
- **Validation**: Allows validation before object creation

### Disadvantages

- **Code Duplication**: Creates duplicate code between the product and builder
- **Complexity**: Adds complexity for simple objects
- **Additional Class**: Requires an extra builder class
- **Mutable Builder**: Builder itself is mutable during construction
- **Object Overhead**: Creates additional objects (the builder)

Comparison with other patterns:

| Aspect | Builder | Factory Method | Abstract Factory |
|--------|---------|---------------|-----------------|
| **Purpose** | Complex object construction | Creating objects of a type | Creating families of objects |
| **Parameters** | Handles many optional parameters | Fixed parameters | Fixed parameters |
| **Flexibility** | Step by step construction | All at once | All at once |
| **Immutability** | Supports immutable objects | Varies | Varies |
| **Use Case** | Objects with many parameters | Type-based object creation | Family-based object creation |

### When to use Builder Pattern:

- When object has many parameters, especially optional ones
- When you need to ensure immutability of constructed objects
- When you want to create different representations of an object
- When construction requires multiple steps
- When you need validation before object creation

[Back to Top](#table-of-contents)

## How does the Builder Pattern differ from the Factory Pattern?

The Builder and Factory patterns serve different purposes in object creation:

### Key Differences

- **Construction Complexity**:
  - **Builder**: Focuses on constructing complex objects step by step
  - **Factory**: Creates objects in a single step

- **Object Creation Process**:
  - **Builder**: Creates objects piece by piece with multiple method calls
  - **Factory**: Creates complete objects with a single method call

- **Parameter Management**:
  - **Builder**: Handles many optional parameters clearly
  - **Factory**: Better for fewer parameters or fixed configurations

- **Method Chaining**:
  - **Builder**: Often uses fluent interface with method chaining
  - **Factory**: Typically doesn't use method chaining

- **Use Case Scenario**:
  - **Builder**: Complex objects with multiple configurable parts
  - **Factory**: Selecting the right subclass to instantiate
  
[Back to Top](#table-of-contents)
