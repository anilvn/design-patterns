# Strategy Pattern

### **Concept of Strategy Pattern**
The **Strategy Pattern** is used to define a family of algorithms, encapsulate each one, and make them interchangeable. It allows a class's behavior to be selected at runtime.

---

### **Implementation**

#### **1. Define the Strategy Interface (`DriveStrategy`)**
```java
// Strategy interface
public interface DriveStrategy {
    void drive();
}
```

#### **2. Implement Concrete Strategies**
```java
// Normal driving strategy
public class NormalDriveStrategy implements DriveStrategy {
    @Override
    public void drive() {
        System.out.println("Driving normally...");
    }
}

// Aggressive driving strategy for sports vehicles
public class SportsDriveStrategy implements DriveStrategy {
    @Override
    public void drive() {
        System.out.println("Driving fast with high acceleration...");
    }
}
```

#### **3. Create the `Vehicle` Abstract Class**
```java
// Vehicle class that uses DriveStrategy
public abstract class Vehicle {
    protected DriveStrategy driveStrategy;

    // Constructor for strategy injection
    public Vehicle(DriveStrategy driveStrategy) {
        this.driveStrategy = driveStrategy;
    }

    public void drive() {
        driveStrategy.drive(); // Delegate driving to the strategy
    }
}
```

#### **4. Implement Concrete Vehicle Types**
```java
// Passenger Vehicle (normal driving)
public class PassengerVehicle extends Vehicle {
    public PassengerVehicle() {
        super(new NormalDriveStrategy()); // Uses normal driving strategy
    }
}

// Sports Vehicle (aggressive driving)
public class SportsVehicle extends Vehicle {
    public SportsVehicle() {
        super(new SportsDriveStrategy()); // Uses sports driving strategy
    }
}
```

#### **5. Testing the Strategy Pattern**
```java
public class StrategyPatternDemo {
    public static void main(String[] args) {
        Vehicle passengerCar = new PassengerVehicle();
        Vehicle sportsCar = new SportsVehicle();

        System.out.println("Passenger Vehicle:");
        passengerCar.drive(); // Output: Driving normally...

        System.out.println("\nSports Vehicle:");
        sportsCar.drive(); // Output: Driving fast with high acceleration...
    }
}
```

---

### **Key Takeaways**
1. **Encapsulation of Behavior** → `DriveStrategy` allows different drive behaviors to be implemented separately.
2. **Interchangeability** → You can swap strategies at runtime if needed.
3. **Open/Closed Principle** → You can introduce new driving behaviors without modifying existing vehicle classes.

---
The **Strategy Design Pattern** is useful when you need to define a family of behaviors (algorithms) and allow the client (context) to choose which one to use dynamically at runtime. It helps **avoid conditional statements** and promotes **code reusability and flexibility**.  

---

## **When to Use the Strategy Pattern?**
### **1. When you have multiple variations of an algorithm**
If a class has multiple behaviors that can change dynamically, use the **Strategy Pattern** instead of `if-else` or `switch-case` statements.

👉 **Example:**  
- Different driving strategies (`NormalDrive`, `SportsDrive`) for vehicles.
- Payment processing (`CreditCardPayment`, `PayPalPayment`).

---

### **2. When behaviors need to be selected at runtime**
If your application allows users to **choose** different behaviors dynamically, the Strategy Pattern helps **swap** implementations at runtime.

👉 **Example:**  
- Choosing different **sorting algorithms** (`QuickSort`, `MergeSort`) based on the dataset.
- Switching between **text compression algorithms** (`ZipCompression`, `RARCompression`).

---

### **3. When you want to follow the Open/Closed Principle (OCP)**
The Strategy Pattern lets you add new behaviors without modifying the existing code, keeping your system **open for extension but closed for modification**.

👉 **Example:**  
- A **game character** with different attack strategies (`SwordAttack`, `BowAttack`).
- A **navigation app** that allows different route-finding strategies (`FastestRoute`, `ShortestRoute`).

---

### **4. When you want to remove duplicate code and improve maintainability**
If multiple classes share similar behavior but with minor differences, using Strategy Pattern helps encapsulate these behaviors in separate classes.

👉 **Example:**  
- Different **authentication mechanisms** (`OAuthAuthentication`, `JWTAuthentication`).
- **Different tax calculation strategies** for different regions.

---

### **5. When you need better testability and scalability**
By isolating the behavior into separate classes, you can **unit test** each strategy independently and easily add new ones without breaking existing code.

👉 **Example:**  
- A **logging system** with multiple logging strategies (`FileLogger`, `DatabaseLogger`, `ConsoleLogger`).

---

## **When NOT to Use Strategy Pattern**
❌ **When the behavior is unlikely to change**  
If the behavior is fixed, using a strategy pattern adds unnecessary complexity.  

❌ **When performance is a concern**  
If the object is frequently changing strategies, it may introduce overhead due to extra object creation.  

❌ **When simple inheritance can solve the problem**  
If a few subclasses can handle variations effectively, subclassing might be a simpler approach than using separate strategy classes.  

---

## **Conclusion**
Use the **Strategy Pattern** when you need to:
✅ Remove conditional logic (`if-else`).  
✅ Support multiple interchangeable behaviors.  
✅ Allow dynamic behavior selection.  
✅ Improve flexibility and maintainability.  