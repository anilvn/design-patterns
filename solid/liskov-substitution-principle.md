
## What is the Liskov Substitution Principle?

The **Liskov Substitution Principle (LSP)** is the third principle in the SOLID design principles.

* LSP states: **"Objects of a superclass should be replaceable with objects of its subclasses without affecting the correctness of the program."**
* In simpler terms, a subclass should not break the behavior expected from the parent class.
* This principle was introduced by Barbara Liskov in 1987.

Key concepts of LSP:

* **Behavioral subtyping**: Subclasses should extend, not replace, the functionality of their parent classes
* **Contract preservation**: Subclasses must honor the contracts established by the parent class
* **Consistent behavior**: A client using a base class should be able to use any of its subclasses without knowing the difference
* **No surprises**: Substituting a base class with a derived class should not produce unexpected behavior

Violations of LSP often occur when:

* Subclasses throw unexpected exceptions
* Subclasses have stricter preconditions
* Subclasses have weaker postconditions
* Subclasses override methods in ways that break expected behavior

Benefits of applying LSP:

* **Code reusability**: Base classes can be extended without breaking existing functionality
* **Reduced bugs**: Fewer unexpected behaviors when substituting objects
* **Better abstraction**: Clearer hierarchies with properly defined parent-child relationships
* **Improved polymorphism**: Objects can be used interchangeably with confidence

LSP is essential for building robust object-oriented systems with reliable inheritance hierarchies.

[Back to Top](#table-of-contents)

## How to fix LSP violations in class hierarchies?

The following example demonstrates fixing a common Liskov Substitution Principle violation in a class hierarchy.

### Violation of LSP:

```java
class Bird {
    public void fly() {
        System.out.println("This bird is flying");
        // Implementation for flying
    }
}

class Sparrow extends Bird {
    // Sparrow can fly, so no issues
    @Override
    public void fly() {
        System.out.println("Sparrow is flying high");
        // Implementation for sparrow flying
    }
}

class Penguin extends Bird {
    @Override
    public void fly() {
        // This violates LSP
        throw new UnsupportedOperationException("Penguins cannot fly!");
    }
}
```

**Problem:**
* If we substitute `Bird` with `Penguin`, it breaks expectations because penguins cannot fly
* Code that expects a `Bird` to fly will fail when given a `Penguin`
* The `Penguin` subclass violates the contract established by the `Bird` class

**Example of the issue:**
```java
public class Main {
    public static void main(String[] args) {
        Bird bird1 = new Sparrow();
        Bird bird2 = new Penguin();
        
        makeAllBirdsFly(bird1); // Works fine
        makeAllBirdsFly(bird2); // Throws exception!
    }
    
    public static void makeAllBirdsFly(Bird bird) {
        // This should work for ALL birds according to LSP
        bird.fly();
    }
    
    /* Sample output:
     * Sparrow is flying high
     * Exception in thread "main" java.lang.UnsupportedOperationException: Penguins cannot fly!
     */
}
```

### LSP-Compliant Solution:

```java
// Base interface for all birds
interface Bird {
    void eat();
}

// Interface for birds that can fly
interface FlyableBird extends Bird {
    void fly();
}

class Sparrow implements FlyableBird {
    @Override
    public void eat() {
        System.out.println("Sparrow is eating...");
        // Implementation for eating
    }

    @Override
    public void fly() {
        System.out.println("Sparrow is flying...");
        // Implementation for flying
    }
}

class Penguin implements Bird {
    @Override
    public void eat() {
        System.out.println("Penguin is eating...");
        // Implementation for eating
    }
    
    // No fly method for penguins
}
```

**Example Usage with LSP-Compliant Design:**
```java
public class Main {
    public static void main(String[] args) {
        // All birds can eat
        Bird sparrow = new Sparrow();
        Bird penguin = new Penguin();
        
        feedBirds(sparrow); // Works fine
        feedBirds(penguin); // Works fine
        
        // Only flyable birds can fly
        FlyableBird flyingSparrow = (FlyableBird) sparrow;
        makeFlyableBirdsFly(flyingSparrow); // Works fine
        
        // We can't even cast penguin to FlyableBird now
        // FlyableBird flyingPenguin = (FlyableBird) penguin; // Compile error!
        
        /* Sample output:
         * Sparrow is eating...
         * Penguin is eating...
         * Sparrow is flying...
         */
    }
    
    public static void feedBirds(Bird bird) {
        // This works for ALL birds
        bird.eat();
    }
    
    public static void makeFlyableBirdsFly(FlyableBird bird) {
        // This works for birds that can fly
        bird.fly();
    }
}
```

**Key Improvements:**
* Proper abstraction: `Bird` interface defines what all birds can do
* Specialized interface: `FlyableBird` extends `Bird` for flying birds
* No forced functionality: `Penguin` is not forced to implement `fly()`
* Clearer hierarchies: Code is more explicit about which birds can fly
* LSP compliant: Substituting a `Bird` with any of its subtypes will not break the program

[Back to Top](#table-of-contents)