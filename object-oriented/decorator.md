# Decorator Pattern 

The Decorator Pattern is a structural design pattern that allows behavior to be added to individual objects dynamically without affecting the behavior of other objects from the same class. It's an alternative to subclassing for extending functionality, following the Open/Closed principle by allowing classes to be open for extension but closed for modification.

In the Decorator Pattern:
- A component interface defines the core functionality
- A concrete component implements the base functionality
- Decorator classes implement the same interface and contain a reference to a component object
- Each decorator adds its behavior before/after delegating to the contained component

This creates a "wrapping" structure where each decorator surrounds the original object, adding layers of functionality.

## Interview Questions About the Decorator Pattern

1. **What's the difference between Decorator and Inheritance for extending functionality?**
   - Decorator adds behavior dynamically at runtime rather than compile time
   - Avoids class explosion with multiple feature combinations
   - Follows composition over inheritance principle
   - Allows functionality to be added/removed during runtime

2. **What are the advantages and disadvantages of the Decorator Pattern?**

   *Advantages:*
   - Adds responsibilities to objects without modifying their code
   - Follows Single Responsibility Principle by separating concerns
   - Greater flexibility than static inheritance
   - Allows combining behaviors in multiple ways

   *Disadvantages:*
   - Can result in many small objects in your design
   - Decorator and its components aren't identical (type issues)
   - Can complicate the code and make it harder to understand
   - Can be difficult to debug with multiple layers of decoration

3. **When would you use the Decorator Pattern vs. Strategy Pattern?**
   - Use Decorator when you want to add responsibilities to objects without subclassing
   - Use Strategy when you want to define a family of interchangeable algorithms
   - Decorator focuses on dynamically enhancing an object, while Strategy focuses on selecting algorithms

4. **How does the Decorator Pattern relate to the Open/Closed Principle?**
   - It enables extending a class's functionality without modifying existing code
   - New features can be added by creating new decorators rather than changing core classes

5. **Can you provide a real-world example where Decorator Pattern would be useful?**
   - File I/O streams (BufferedInputStream, DataInputStream)
   - UI component enhancement (adding scrolling, borders)
   - Pizza topping additions
   - Coffee customization systems (adding milk, sugar, etc.)
6. **How is the Decorator Pattern different from the Proxy Pattern?**

---
Here's an example of the **Decorator Design Pattern** in Java. The **Decorator Pattern** is used to dynamically add behavior to an object without modifying its existing code.

---

### **Example: Coffee Shop**
In this example, we have a base `Coffee` interface, a concrete class `SimpleCoffee`, and multiple decorators like `Milk` and `Sugar` that add additional functionality.

---

#### **Step 1: Create the Base Component Interface**
```java
// Component Interface
interface Coffee {
    String getDescription();
    double getCost();
}
```

---

#### **Step 2: Create a Concrete Component**
```java
// Concrete Component
class SimpleCoffee implements Coffee {
    @Override
    public String getDescription() {
        return "Simple Coffee";
    }

    @Override
    public double getCost() {
        return 5.0;
    }
}
```

---

#### **Step 3: Create an Abstract Decorator Class**
```java
// Decorator Abstract Class
abstract class CoffeeDecorator implements Coffee {
    protected Coffee decoratedCoffee;

    public CoffeeDecorator(Coffee coffee) {
        this.decoratedCoffee = coffee;
    }

    @Override
    public String getDescription() {
        return decoratedCoffee.getDescription();
    }

    @Override
    public double getCost() {
        return decoratedCoffee.getCost();
    }
}
```

---

#### **Step 4: Create Concrete Decorators**
```java
// Concrete Decorator 1: Adding Milk
class Milk extends CoffeeDecorator {
    public Milk(Coffee coffee) {
        super(coffee);
    }

    @Override
    public String getDescription() {
        return super.getDescription() + ", Milk";
    }

    @Override
    public double getCost() {
        return super.getCost() + 2.0; // Adding milk costs extra
    }
}

// Concrete Decorator 2: Adding Sugar
class Sugar extends CoffeeDecorator {
    public Sugar(Coffee coffee) {
        super(coffee);
    }

    @Override
    public String getDescription() {
        return super.getDescription() + ", Sugar";
    }

    @Override
    public double getCost() {
        return super.getCost() + 1.0; // Adding sugar costs extra
    }
}
```

---

#### **Step 5: Use the Decorator Pattern**
```java
public class DecoratorPatternExample {
    public static void main(String[] args) {
        Coffee coffee = new SimpleCoffee();
        System.out.println(coffee.getDescription() + " -> Cost: $" + coffee.getCost());

        // Adding Milk
        coffee = new Milk(coffee);
        System.out.println(coffee.getDescription() + " -> Cost: $" + coffee.getCost());

        // Adding Sugar
        coffee = new Sugar(coffee);
        System.out.println(coffee.getDescription() + " -> Cost: $" + coffee.getCost());

        // Adding More Sugar
        coffee = new Sugar(coffee);
        System.out.println(coffee.getDescription() + " -> Cost: $" + coffee.getCost());
    }
}
```

---

### **Output:**
```
Simple Coffee -> Cost: $5.0
Simple Coffee, Milk -> Cost: $7.0
Simple Coffee, Milk, Sugar -> Cost: $8.0
Simple Coffee, Milk, Sugar, Sugar -> Cost: $9.0
```

---

### **Explanation:**
1. We start with a `SimpleCoffee` that costs **$5.0**.
2. We decorate it with `Milk`, increasing the cost to **$7.0**.
3. We add `Sugar`, making the total cost **$8.0**.
4. We add another `Sugar`, and now the total is **$9.0**.

This demonstrates how the **Decorator Pattern** allows us to add behavior dynamically without modifying existing classes. 🚀


---


## **Decorator Pattern: Pizza Example**
We have:
1. **BasePizza** (Abstract component)
2. **Concrete Pizza Types** (Farmhouse, VegDelight, Margherita)
3. **Toppings (Decorators)** (ExtraCheese, Mushroom)

---

### **Step 1: Base Component - `BasePizza`**
```java
// Abstract Component
public abstract class BasePizza {
    public abstract int cost();
}
```

---

### **Step 2: Concrete Components - Different Pizza Types**
```java
// Concrete Component: Farmhouse Pizza
public class Farmhouse extends BasePizza {
    @Override
    public int cost() {
        return 200;
    }
}

// Concrete Component: Veg Delight Pizza
public class VegDelight extends BasePizza {
    @Override
    public int cost() {
        return 120;
    }
}

// Concrete Component: Margherita Pizza
public class Margherita extends BasePizza {
    @Override
    public int cost() {
        return 100;
    }
}
```

---

### **Step 3: Abstract Decorator - `ToppingDecorator`**
```java
// Abstract Decorator
public abstract class ToppingDecorator extends BasePizza {
    protected BasePizza basePizza;

    public ToppingDecorator(BasePizza pizza) {
        this.basePizza = pizza;
    }

    @Override
    public abstract int cost();
}
```

---

### **Step 4: Concrete Decorators - Different Toppings**
```java
// Concrete Decorator: Extra Cheese
public class ExtraCheese extends ToppingDecorator {
    public ExtraCheese(BasePizza pizza) {
        super(pizza);
    }

    @Override
    public int cost() {
        return basePizza.cost() + 18; // Adding extra cheese costs 18
    }
}

// Concrete Decorator: Mushroom
public class Mushroom extends ToppingDecorator {
    public Mushroom(BasePizza pizza) {
        super(pizza);
    }

    @Override
    public int cost() {
        return basePizza.cost() + 15; // Adding mushroom costs 15
    }
}
```

---

### **Step 5: Test the Decorator Pattern**
```java
public class PizzaDecoratorDemo {
    public static void main(String[] args) {
        // Order a Farmhouse pizza
        BasePizza pizza = new Farmhouse();
        System.out.println("Farmhouse Pizza Cost: " + pizza.cost());

        // Add Extra Cheese to Farmhouse Pizza
        pizza = new ExtraCheese(pizza);
        System.out.println("Farmhouse with Extra Cheese Cost: " + pizza.cost());

        // Add Mushroom on top of Extra Cheese
        pizza = new Mushroom(pizza);
        System.out.println("Farmhouse with Extra Cheese & Mushroom Cost: " + pizza.cost());

        // Order a Margherita pizza with Extra Cheese
        BasePizza margheritaPizza = new ExtraCheese(new Margherita());
        System.out.println("Margherita with Extra Cheese Cost: " + margheritaPizza.cost());
    }
}
```

---

### **Output:**
```
Farmhouse Pizza Cost: 200
Farmhouse with Extra Cheese Cost: 218
Farmhouse with Extra Cheese & Mushroom Cost: 233
Margherita with Extra Cheese Cost: 118
```

---

### **Explanation:**
1. We start with a **Farmhouse Pizza** (₹200).
2. Adding **Extra Cheese** increases cost to **₹218**.
3. Adding **Mushrooms** on top of Extra Cheese increases cost to **₹233**.
4. Ordering a **Margherita Pizza** (₹100) with **Extra Cheese** costs **₹118**.

---

### **Why Use the Decorator Pattern?**
✔ **Flexible:** We can add toppings dynamically.  
✔ **Open-Closed Principle:** New toppings can be added without modifying existing pizza classes.  
✔ **Reusability:** Toppings (decorators) can be applied to multiple pizza types.  

🚀 Now you can easily add more toppings like **Olives, Jalapenos, or Paneer** using the same pattern! 🎉