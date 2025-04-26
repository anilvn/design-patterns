# Prototype Design Pattern 

## What is the Prototype Design Pattern?

The Prototype pattern is a creational design pattern that allows cloning objects without coupling to their specific classes, essentially creating new objects by copying existing ones.

Key features:
- **Object Copying**: Creates new objects by copying existing ones
- **Avoid Subclassing**: Creates objects without depending on their classes
- **Runtime Configuration**: Adds and removes objects at runtime
- **Reduced Initialization Overhead**: Avoids costly initialization processes
- **Clone Interface**: Uses a common interface for all prototypes

## **Prototype Pattern with a Simple Shape Object**

```java
// Prototype Interface
interface Prototype {
    Prototype clone();
    void customize(String color);
    void display();
}

// Concrete Prototype
class Circle implements Prototype {
    private int radius;
    private String color;
    
    public Circle(int radius, String color) {
        this.radius = radius;
        this.color = color;
        System.out.println("Creating a Circle...");
    }
    
    // Private constructor for cloning
    private Circle(Circle source) {
        this.radius = source.radius;
        this.color = source.color;
    }
    
    @Override
    public Prototype clone() {
        return new Circle(this);
    }
    
    @Override
    public void customize(String newColor) {
        this.color = newColor;
    }
    
    @Override
    public void display() {
        System.out.println("Circle [Radius: " + radius + ", Color: " + color + "]");
    }
}
```

---

### ✅ **Usage Example**

```java
public class PrototypeSimpleDemo {
    public static void main(String[] args) {
        // Create original Circle
        Prototype originalCircle = new Circle(5, "Red");
        
        System.out.println("\nOriginal Circle:");
        originalCircle.display();
        
        // Create clones
        Prototype copy1 = originalCircle.clone();
        copy1.customize("Blue");
        
        Prototype copy2 = originalCircle.clone();
        copy2.customize("Green");
        
        System.out.println("\nCopy 1:");
        copy1.display();
        
        System.out.println("\nCopy 2:");
        copy2.display();
    }
}
```

---

### ✅ **Output**

```
Creating a Circle...

Original Circle:
Circle [Radius: 5, Color: Red]

Copy 1:
Circle [Radius: 5, Color: Blue]

Copy 2:
Circle [Radius: 5, Color: Green]
```

---

## 📌 **Summary**
- **Original object** is created once (expensive if needed).
- **Clones** are created quickly by **copying** the original.
- We can **customize cloned objects** without touching the original.
- **Prototype Pattern** is best when **object creation is costly** and you **want to avoid repeating it**.


## How to implement the Prototype Pattern in Java?

Java provides built-in support for the Prototype pattern through the `Cloneable` interface and `Object.clone()` method. There are also custom implementations that may offer more control.

### Using Java's Cloneable Interface

```java
import java.util.ArrayList;
import java.util.List;

// Prototype using Cloneable interface
class Employee implements Cloneable {
    private String name;
    private String department;
    private List<String> skills;
    
    public Employee(String name, String department) {
        this.name = name;
        this.department = department;
        this.skills = new ArrayList<>();
    }
    
    // Copy constructor (useful for deep copies)
    public Employee(Employee source) {
        this.name = source.name;
        this.department = source.department;
        // Deep copy of skills list
        this.skills = new ArrayList<>(source.skills);
    }
    
    public void addSkill(String skill) {
        skills.add(skill);
    }
    
    public String getName() {
        return name;
    }
    
    public void setName(String name) {
        this.name = name;
    }
    
    public String getDepartment() {
        return department;
    }
    
    public void setDepartment(String department) {
        this.department = department;
    }
    
    public List<String> getSkills() {
        return skills;
    }
    
    @Override
    public String toString() {
        return "Employee [name=" + name + 
               ", department=" + department + 
               ", skills=" + skills + "]";
    }
    
    // Implementing clone() from Cloneable interface
    @Override
    public Employee clone() {
        try {
            // Shallow copy
            Employee clone = (Employee) super.clone();
            // Deep copy
            clone.skills = new ArrayList<>(this.skills);
            return clone;
        } catch (CloneNotSupportedException e) {
            // This shouldn't happen since we implement Cloneable
            throw new AssertionError("Clone not supported: " + e);
        }
    }
}
```

Usage example:
```java
public class CloneableDemo {
    public static void main(String[] args) {
        // Create original employee
        Employee original = new Employee("John Doe", "Engineering");
        original.addSkill("Java");
        original.addSkill("Spring");
        original.addSkill("Docker");
        
        System.out.println("Original: " + original);
        
        // Clone the employee (method 1: using clone())
        Employee clone1 = original.clone();
        clone1.setName("Jane Smith");
        clone1.addSkill("Kubernetes");
        
        // Clone the employee (method 2: using copy constructor)
        Employee clone2 = new Employee(original);
        clone2.setName("Bob Johnson");
        clone2.setDepartment("DevOps");
        clone2.getSkills().remove("Spring");
        
        // Show all employees
        System.out.println("Original after cloning: " + original);
        System.out.println("Clone 1: " + clone1);
        System.out.println("Clone 2: " + clone2);
        
        // Output:
        // Original: Employee [name=John Doe, department=Engineering, skills=[Java, Spring, Docker]]
        // Original after cloning: Employee [name=John Doe, department=Engineering, skills=[Java, Spring, Docker]]
        // Clone 1: Employee [name=Jane Smith, department=Engineering, skills=[Java, Spring, Docker, Kubernetes]]
        // Clone 2: Employee [name=Bob Johnson, department=DevOps, skills=[Java, Docker]]
    }
}
```

### Prototype Registry Implementation

A prototype registry maintains a collection of ready-to-use prototypes:

```java
import java.util.HashMap;
import java.util.Map;

// Prototype interface
interface Shape extends Cloneable {
    Shape clone();
    void draw();
    void setColor(String color);
    String getColor();
}

// Concrete prototype: Circle
class Circle implements Shape {
    private String color;
    private int radius;
    
    public Circle(int radius) {
        this.radius = radius;
        // Simulate expensive resource initialization
        try {
            Thread.sleep(100);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
    
    // Private constructor for cloning
    private Circle(Circle source) {
        this.radius = source.radius;
        this.color = source.color;
    }
    
    @Override
    public Shape clone() {
        return new Circle(this);
    }
    
    @Override
    public void draw() {
        System.out.println("Drawing a " + color + " circle with radius " + radius);
    }
    
    @Override
    public void setColor(String color) {
        this.color = color;
    }
    
    @Override
    public String getColor() {
        return color;
    }
}

// Concrete prototype: Rectangle
class Rectangle implements Shape {
    private String color;
    private int width;
    private int height;
    
    public Rectangle(int width, int height) {
        this.width = width;
        this.height = height;
        // Simulate expensive resource initialization
        try {
            Thread.sleep(100);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
    
    // Private constructor for cloning
    private Rectangle(Rectangle source) {
        this.width = source.width;
        this.height = source.height;
        this.color = source.color;
    }
    
    @Override
    public Shape clone() {
        return new Rectangle(this);
    }
    
    @Override
    public void draw() {
        System.out.println("Drawing a " + color + " rectangle with width " + width + " and height " + height);
    }
    
    @Override
    public void setColor(String color) {
        this.color = color;
    }
    
    @Override
    public String getColor() {
        return color;
    }
}

// Prototype registry
class ShapeRegistry {
    private Map<String, Shape> shapes = new HashMap<>();
    
    public void addShape(String key, Shape shape) {
        shapes.put(key, shape);
    }
    
    public Shape getShape(String key) {
        Shape shape = shapes.get(key);
        if (shape == null) {
            return null;
        }
        // Return a clone, not the original
        return shape.clone();
    }
    
    public void loadInitialShapes() {
        System.out.println("Loading initial shapes (expensive operation)...");
        
        Circle circle = new Circle(10);
        circle.setColor("Red");
        shapes.put("smallCircle", circle);
        
        Circle largeCircle = new Circle(100);
        largeCircle.setColor("Blue");
        shapes.put("largeCircle", largeCircle);
        
        Rectangle rectangle = new Rectangle(20, 30);
        rectangle.setColor("Green");
        shapes.put("rectangle", rectangle);
        
        System.out.println("Initial shapes loaded.");
    }
}
```

Usage of prototype registry:
```java
public class PrototypeRegistryDemo {
    public static void main(String[] args) {
        // Setup the registry with initial prototypes (expensive operation)
        ShapeRegistry registry = new ShapeRegistry();
        registry.loadInitialShapes();
        
        // Use the registry to clone objects (fast operation)
        System.out.println("\nCreating shapes from prototypes:");
        
        // Clone small circle and modify it
        Shape circle1 = registry.getShape("smallCircle");
        circle1.setColor("Yellow");
        circle1.draw();
        
        // Clone another small circle with different color
        Shape circle2 = registry.getShape("smallCircle");
        circle2.setColor("Purple");
        circle2.draw();
        
        // Clone large circle
        Shape largeCircle = registry.getShape("largeCircle");
        largeCircle.draw();
        
        // Clone rectangle
        Shape rectangle = registry.getShape("rectangle");
        rectangle.draw();
        
        // Output:
        // Loading initial shapes (expensive operation)...
        // Initial shapes loaded.
        //
        // Creating shapes from prototypes:
        // Drawing a Yellow circle with radius 10
        // Drawing a Purple circle with radius 10
        // Drawing a Blue circle with radius 100
        // Drawing a Green rectangle with width 20 and height 30
    }
}
```

[Back to Top](#table-of-contents)

## What are the advantages and disadvantages of Prototype Pattern?

### Advantages

- **Reduced Subclassing**: Creates objects without relying on class hierarchies
- **Runtime Configuration**: Add/remove prototypes at runtime
- **Performance Improvement**: Avoiding costly initialization processes
- **Dynamic Object Creation**: Configures and instantiates objects at runtime
- **Complex Object Creation**: Simplifies creation of complex objects

### Disadvantages

- **Cloning Complexity**: Deep copying can be complex for objects with circular references
- **Clone Method Implementation**: Each concrete prototype must implement the clone operation
- **Initialization Issues**: Clones might share state when objects contain references
- **Shallow vs Deep Copy**: Need to decide between shallow and deep copying
- **Learning Curve**: Understanding the nuances of object cloning

Comparison with other patterns:

| Aspect | Prototype | Factory | Builder |
|--------|-----------|---------|---------|
| **Creation Mechanism** | Cloning existing objects | Creating new instances | Step-by-step construction |
| **Complexity** | Medium | Low | High |
| **Use Case** | Avoid costly initialization | Create objects of a hierarchy | Construct complex objects |
| **Runtime Change** | Supports runtime changes | Limited | Limited |
| **Performance** | Better for expensive initialization | Standard | Standard |

### When to use Prototype Pattern:

- When object creation is expensive (complex queries, file loading)
- When classes to instantiate are specified at runtime
- When avoiding building a class hierarchy of factories
- When instances can have only a few combinations of states
- When objects need to be created dynamically

[Back to Top](#table-of-contents)

## How does Prototype Pattern handle deep cloning?

Deep cloning is a critical aspect of the Prototype pattern when dealing with objects that contain references to other objects.

### Deep vs. Shallow Cloning

- **Shallow Clone**: Copies the object's fields but keeps references to the same objects
- **Deep Clone**: Recursively copies all objects referenced by the original object

### Methods for Deep Cloning

1. **Manual Deep Cloning**
   - Implement custom logic to recursively clone all nested objects
   - Most control but most verbose

2. **Serialization-based Cloning**
   - Use Java serialization to create deep copies
   - Convenient but less performant

3. **Copy Constructors**
   - Create a constructor that takes an instance of the same class
   - Simple approach for controlled deep copying

4. **Clone with Object Recursion**
   - Override `clone()` and recursively clone contained objects
   - Good middle ground approach

```java
import java.io.*;
import java.util.ArrayList;
import java.util.List;

// Class to be contained in another class
class Address implements Serializable {
    private static final long serialVersionUID = 1L;
    private String street;
    private String city;
    private String country;
    
    public Address(String street, String city, String country) {
        this.street = street;
        this.city = city;
        this.country = country;
    }
    
    // Copy constructor for deep copying
    public Address(Address source) {
        this.street = source.street;
        this.city = source.city;
        this.country = source.country;
    }
    
    @Override
    public String toString() {
        return street + ", " + city + ", " + country;
    }
    
    // Getters and setters
    public String getStreet() { return street; }
    public void setStreet(String street) { this.street = street; }
    public String getCity() { return city; }
    public void setCity(String city) { this.city = city; }
    public String getCountry() { return country; }
    public void setCountry(String country) { this.country = country; }
}

// Main class implementing different cloning approaches
class Person implements Cloneable, Serializable {
    private static final long serialVersionUID = 2L;
    private String name;
    private int age;
    private Address address;         // Reference type
    private List<String> hobbies;    // Collection of strings
    
    public Person(String name, int age, Address address) {
        this.name = name;
        this.age = age;
        this.address = address;
        this.hobbies = new ArrayList<>();
    }
    
    public void addHobby(String hobby) {
        hobbies.add(hobby);
    }
    
    @Override
    public String toString() {
        return "Person [name=" + name + 
               ", age=" + age + 
               ", address=" + address + 
               ", hobbies=" + hobbies + "]";
    }
    
    // Getters and setters
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public int getAge() { return age; }
    public void setAge(int age) { this.age = age; }
    public Address getAddress() { return address; }
    public void setAddress(Address address) { this.address = address; }
    public List<String> getHobbies() { return hobbies; }
    
    // Method 1: Manual Deep Clone Implementation
    @Override
    public Person clone() {
        try {
            // Shallow copy via super.clone()
            Person cloned = (Person) super.clone();
            
            // Deep copy of reference fields
            cloned.address = new Address(this.address); // Using copy constructor
            cloned.hobbies = new ArrayList<>(this.hobbies); // Copy ArrayList
            
            return cloned;
        } catch (CloneNotSupportedException e) {
            throw new AssertionError("Clone not supported: " + e);
        }
    }
    
    // Method 2: Serialization-based Deep Clone
    public Person deepCopyBySerialization() {
        try {
            // Write the object to a byte array
            ByteArrayOutputStream bos = new ByteArrayOutputStream();
            ObjectOutputStream out = new ObjectOutputStream(bos);
            out.writeObject(this);
            out.flush();
            
            // Create a new object by reading from byte array
            ByteArrayInputStream bis = new ByteArrayInputStream(bos.toByteArray());
            ObjectInputStream in = new ObjectInputStream(bis);
            Person copied = (Person) in.readObject();
            
            // Close streams
            out.close();
            in.close();
            
            return copied;
        } catch (IOException | ClassNotFoundException e) {
            throw new RuntimeException("Error during serialization/deserialization", e);
        }
    }
    
    // Method 3: Copy Constructor
    public Person(Person source) {
        this.name = source.name;
        this.age = source.age;
        // Deep copy address
        this.address = new Address(source.address);
        // Deep copy hobbies
        this.hobbies = new ArrayList<>(source.hobbies);
    }
}
```

Testing deep cloning:
```java
public class DeepCloningDemo {
    public static void main(String[] args) {
        // Create original person
        Address address = new Address("123 Main St", "New York", "USA");
        Person original = new Person("John", 30, address);
        original.addHobby("Reading");
        original.addHobby("Swimming");
        
        System.out.println("Original person: " + original);
        
        // Method 1: Clone using override of clone()
        Person clone1 = original.clone();
        testClone("Method 1 (Override clone)", original, clone1);
        
        // Method 2: Clone using serialization
        Person clone2 = original.deepCopyBySerialization();
        testClone("Method 2 (Serialization)", original, clone2);
        
        // Method 3: Clone using copy constructor
        Person clone3 = new Person(original);
        testClone("Method 3 (Copy Constructor)", original, clone3);
    }
    
    private static void testClone(String method, Person original, Person clone) {
        System.out.println("\n=== Testing " + method + " ===");
        
        // Test reference equality
        System.out.println("Same instance? " + (original == clone));
        System.out.println("Before modification:");
        System.out.println("Original: " + original);
        System.out.println("Clone:    " + clone);
        
        // Modify clone and check if original is affected
        clone.setName("Modified");
        clone.setAge(40);
        clone.getAddress().setStreet("456 Park Ave");
        clone.getHobbies().add("Cooking");
        
        System.out.println("\nAfter modification:");
        System.out.println("Original: " + original);
        System.out.println("Clone:    " + clone);
        
        // Output for each method should show that original is unaffected by changes to the clone
    }
}
```

Performance comparison:

| Cloning Method | Advantages | Disadvantages | Performance |
|----------------|------------|--------------|-------------|
| **Manual Deep Clone** | Precise control, Performance | Verbose, Error-prone | Fast |
| **Serialization** | Simple to implement, Handles all object graphs | Slow, Requires Serializable | Slow |
| **Copy Constructor** | Simple, Clear, No exceptions | Needs maintenance with class changes | Fast |

[Back to Top](#table-of-contents)
