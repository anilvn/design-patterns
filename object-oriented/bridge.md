# Bridge Pattern

### ✅ **Bridge Design Pattern – Definition:**

> The **Bridge Design Pattern** is a structural design pattern that **decouples an abstraction from its implementation**, so that the two can **vary independently**.

---

### 🔍 **In Simple Terms:**
It allows you to separate what an object does (**abstraction**) from how it does it (**implementation**) by using **composition** instead of inheritance. This makes it easier to **extend** both the abstraction and the implementation **without changing existing code**.

---

### 📦 **Key Components:**
- **Abstraction**: Defines the interface and maintains a reference to the implementor.
- **Refined Abstraction**: Extends the abstraction interface.
- **Implementor**: Defines the interface for implementation classes.
- **ConcreteImplementor**: Implements the `Implementor` interface.

---

### 🧠 **When to Use:**
- When you want to avoid a permanent binding between an abstraction and its implementation.
- When both the abstraction and implementation may need to be extended independently.
- When you have a proliferation of classes due to multiple dimensions of variation (e.g., shape types and rendering APIs).

---

Would you like a visual diagram of how the Bridge Pattern works?

### Ex Shows without using the Bridge Pattern

```java
// Abstraction
public abstract class LivingThings {
    protected BreatheImplementor breatheImplementor;

    public LivingThings(BreatheImplementor breatheImplementor) {
        this.breatheImplementor = breatheImplementor;
    }

    public abstract void breatheProcess();
}

// Implementor Interface
public interface BreatheImplementor {
    void breathe();
}

// Concrete Implementations of BreatheImplementor
public class LandBreatheImplementation implements BreatheImplementor {
    @Override
    public void breathe() {
        System.out.println("Breathing through NOSE");
        System.out.println("Inhaling Oxygen from Air");
        System.out.println("Exhaling Carbon Dioxide");
    }
}

public class WaterBreatheImplementation implements BreatheImplementor {
    @Override
    public void breathe() {
        System.out.println("Breathing through GILLS");
        System.out.println("Absorbing Oxygen from water");
        System.out.println("Releasing Carbon Dioxide");
    }
}

public class TreeBreatheImplementation implements BreatheImplementor {
    @Override
    public void breathe() {
        System.out.println("Breathing through LEAVES");
        System.out.println("Inhaling Carbon Dioxide");
        System.out.println("Exhaling Oxygen through Photosynthesis");
    }
}

// Concrete Classes
public class Fish extends LivingThings {
    public Fish(BreatheImplementor breatheImplementor) {
        super(breatheImplementor);
    }

    @Override
    public void breatheProcess() {
        System.out.println("Fish breathing process:");
        breatheImplementor.breathe();
    }
}

public class Tree extends LivingThings {
    public Tree(BreatheImplementor breatheImplementor) {
        super(breatheImplementor);
    }

    @Override
    public void breatheProcess() {
        System.out.println("Tree breathing process:");
        breatheImplementor.breathe();
    }
}

// Main Class to Test
public class Main {
    public static void main(String[] args) {
        LivingThings fish = new Fish(new WaterBreatheImplementation());
        fish.breatheProcess();

        System.out.println();

        LivingThings tree = new Tree(new TreeBreatheImplementation());
        tree.breatheProcess();
    }
}
```
### **Output**
```
Fish breathing process:
Breathing through GILLS
Absorbing Oxygen from water
Releasing Carbon Dioxide

Tree breathing process:
Breathing through LEAVES
Inhaling Carbon Dioxide
Exhaling Oxygen through Photosynthesis
```

### **Explanation**
1. **Abstraction (`LivingThings`)**: Holds a reference to `BreatheImplementor`, allowing different breathing mechanisms.
2. **Implementor (`BreatheImplementor`)**: Defines a common interface for breathing.
3. **Concrete Implementations (`LandBreatheImplementation`, `WaterBreatheImplementation`, `TreeBreatheImplementation`)**: Implement different breathing processes.
4. **Concrete Classes (`Fish`, `Tree`)**: Extend `LivingThings` and delegate the breathing process to the `BreatheImplementor`.


<br/><br/><br/><br/>

## **Problem Statement**
In nature, different living things have different breathing mechanisms. For example:
- **Fish** breathe through **gills**, absorbing oxygen from water.
- **Trees** breathe through **leaves**, inhaling carbon dioxide and releasing oxygen.
- **Humans and land animals** breathe through **nose/lungs**, inhaling oxygen from the air.

If we directly implement these breathing mechanisms inside each class (`Fish`, `Tree`, etc.), it would lead to **tight coupling**, meaning:
1. **Code Duplication**: Each class would implement its own breathing method, leading to redundant code.
2. **Difficult Maintenance**: If a new breathing type is introduced, we need to modify multiple classes.
3. **Limited Flexibility**: We cannot easily assign different breathing behaviors to objects dynamically.

## **Solution Using Bridge Design Pattern**
The **Bridge Pattern** helps us separate the **abstraction (LivingThings)** from the **implementation (breathing process)**, making the code more flexible and maintainable.

- **Step 1: Create an Abstract Class (`LivingThings`)**  
  - This class represents all living things but delegates breathing to a separate implementation.

- **Step 2: Create a `BreatheImplementor` Interface**  
  - This interface defines the breathing mechanism.

- **Step 3: Implement Different Breathing Behaviors**  
  - `LandBreatheImplementation`, `WaterBreatheImplementation`, and `TreeBreatheImplementation` implement the `BreatheImplementor` interface.

- **Step 4: Extend `LivingThings` for Specific Classes (`Fish`, `Tree`)**  
  - These classes delegate their breathing processes to the appropriate `BreatheImplementor`.

---

## **Implementation Using Bridge Pattern**
```java
// **Step 1: Abstract Class (Abstraction)**
public abstract class LivingThings {
    protected BreatheImplementor breatheImplementor;

    public LivingThings(BreatheImplementor breatheImplementor) {
        this.breatheImplementor = breatheImplementor;
    }

    public abstract void breatheProcess();
}

// **Step 2: Implementor Interface**
public interface BreatheImplementor {
    void breathe();
}

// **Step 3: Concrete Implementations of Breathing Mechanism**
public class LandBreatheImplementation implements BreatheImplementor {
    @Override
    public void breathe() {
        System.out.println("Breathing through NOSE");
        System.out.println("Inhaling Oxygen from Air");
        System.out.println("Exhaling Carbon Dioxide");
    }
}

public class WaterBreatheImplementation implements BreatheImplementor {
    @Override
    public void breathe() {
        System.out.println("Breathing through GILLS");
        System.out.println("Absorbing Oxygen from water");
        System.out.println("Releasing Carbon Dioxide");
    }
}

public class TreeBreatheImplementation implements BreatheImplementor {
    @Override
    public void breathe() {
        System.out.println("Breathing through LEAVES");
        System.out.println("Inhaling Carbon Dioxide");
        System.out.println("Exhaling Oxygen through Photosynthesis");
    }
}

// **Step 4: Concrete LivingThings Classes**
public class Fish extends LivingThings {
    public Fish(BreatheImplementor breatheImplementor) {
        super(breatheImplementor);
    }

    @Override
    public void breatheProcess() {
        System.out.println("Fish breathing process:");
        breatheImplementor.breathe();
    }
}

public class Tree extends LivingThings {
    public Tree(BreatheImplementor breatheImplementor) {
        super(breatheImplementor);
    }

    @Override
    public void breatheProcess() {
        System.out.println("Tree breathing process:");
        breatheImplementor.breathe();
    }
}

// **Step 5: Main Class to Test the Implementation**
public class Main {
    public static void main(String[] args) {
        LivingThings fish = new Fish(new WaterBreatheImplementation());
        fish.breatheProcess();

        System.out.println();

        LivingThings tree = new Tree(new TreeBreatheImplementation());
        tree.breatheProcess();
    }
}
```

## **Output of the Program**
```
Fish breathing process:
Breathing through GILLS
Absorbing Oxygen from water
Releasing Carbon Dioxide

Tree breathing process:
Breathing through LEAVES
Inhaling Carbon Dioxide
Exhaling Oxygen through Photosynthesis
```

---

## **How Bridge Pattern Solves the Problem**
✅ **Decoupling**: The `LivingThings` class doesn't directly implement breathing mechanisms. Instead, it **delegates** the responsibility to `BreatheImplementor`, making it more modular.  

✅ **Reusability**: If we need a new breathing method (e.g., skin breathing for amphibians), we just create another `BreatheImplementor` class without modifying existing classes.  

✅ **Scalability**: We can add more `LivingThings` (like `Human`, `Amphibian`) or more breathing behaviors without affecting existing code.  

---



This implementation follows the **Bridge Design Pattern**, making the code **more flexible, scalable, and maintainable**. 🚀


