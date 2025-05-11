
# Flyweight Pattern

### 🔹 **Flyweight Design Pattern – Definition**

The **Flyweight Design Pattern** is a **structural design pattern** that enables efficient memory usage by **sharing common (intrinsic) data among multiple objects**, instead of storing duplicate data in each object. It is particularly useful when a large number of similar objects are created, and the cost of storing them individually is high.

In this pattern:
- **Intrinsic state** is shared and stored in a single shared object (e.g., character shape, font).
- **Extrinsic state** (e.g., position, color) is passed from the client and varies between objects.

---

### ✅ **Use When**:
- You need to create **a large number of similar objects**.
- **Memory consumption is a concern**.
- Shared data between objects is **static and reusable**.

---

### 📌 Real-world Examples:
- Characters in a **word processor** (e.g., sharing font styles).
- **Game entities** like enemies that share textures but have different positions.

The **Flyweight Design Pattern** is useful for optimizing memory usage by sharing common object data instead of creating redundant instances.

---

### **Key Fixes & Improvements:**
1. **Syntax Issues Fixed:**
   - Fixed missing brackets (`{}`), missing `;`, and incorrect method declarations.
   - Replaced incorrectly formatted parameters like `robotType: "HUMANOID"` with `robotType, "HUMANOID"`.

2. **Corrected Flyweight Factory Logic:**
   - Added correct `if-else` structure.
   - Fixed the **caching mechanism** to avoid incorrect reuse.
   - Standardized key names in the cache (`roboticObjectCache`) to prevent mismatches.

3. **Ensured Proper Display Implementation:**
   - Added `display(int x, int y)` method logic.

---

### **Updated Flyweight Pattern Implementation in Java**
```java
import java.util.HashMap;
import java.util.Map;

// Flyweight Interface
interface IRobot {
    void display(int x, int y);
}

// Concrete Flyweight for Humanoid Robots
class HumanoidRobot implements IRobot {
    private final String type;
    private final Sprites body; // Intrinsic shared data

    HumanoidRobot(String type, Sprites body) {
        this.type = type;
        this.body = body;
    }

    @Override
    public void display(int x, int y) {
        System.out.println(type + " displayed at coordinates (" + x + ", " + y + ")");
    }
}

// Concrete Flyweight for Robotic Dogs
class RoboticDog implements IRobot {
    private final String type;
    private final Sprites body; // Intrinsic shared data

    RoboticDog(String type, Sprites body) {
        this.type = type;
        this.body = body;
    }

    @Override
    public void display(int x, int y) {
        System.out.println(type + " displayed at coordinates (" + x + ", " + y + ")");
    }
}

// Flyweight Factory
class RoboticFactory {
    private static final Map<String, IRobot> roboticObjectCache = new HashMap<>();

    public static IRobot createRobot(String robotType) {
        if (roboticObjectCache.containsKey(robotType)) {
            return roboticObjectCache.get(robotType);
        } else {
            IRobot robot;
            if (robotType.equals("HUMANOID")) {
                robot = new HumanoidRobot("HUMANOID", new Sprites());
            } else if (robotType.equals("ROBOTIC_DOG")) {
                robot = new RoboticDog("ROBOTIC_DOG", new Sprites());
            } else {
                return null;
            }
            roboticObjectCache.put(robotType, robot);
            return robot;
        }
    }
}

// Supporting Sprites Class (Shared Data)
class Sprites {
    // Assume this class contains graphical representation logic
}

// Client Code
public class Main {
    public static void main(String[] args) {
        IRobot humanoidRobot1 = RoboticFactory.createRobot("HUMANOID");
        humanoidRobot1.display(1, 2);

        IRobot humanoidRobot2 = RoboticFactory.createRobot("HUMANOID");
        humanoidRobot2.display(10, 30);

        IRobot roboDog1 = RoboticFactory.createRobot("ROBOTIC_DOG");
        roboDog1.display(2, 9);

        IRobot roboDog2 = RoboticFactory.createRobot("ROBOTIC_DOG");
        roboDog2.display(11, 19);

        // Verifying that objects are shared
        System.out.println("humanoidRobot1 == humanoidRobot2: " + (humanoidRobot1 == humanoidRobot2));
        System.out.println("roboDog1 == roboDog2: " + (roboDog1 == roboDog2));
    }
}
```

---

### **How This Implements the Flyweight Pattern**
1. **Intrinsic Data (Shared)**  
   - `Sprites` object is shared among multiple robots to reduce memory usage.
   - Robot types (`HUMANOID`, `ROBOTIC_DOG`) remain the same across instances.

2. **Extrinsic Data (Variable)**
   - The `display(int x, int y)` method takes `x, y` coordinates as external input.
   - Each robot object does not store unique `x, y` values but receives them at runtime.

3. **Flyweight Factory (`RoboticFactory`)**
   - Uses a **cache (`HashMap<String, IRobot> roboticObjectCache`)** to store and reuse shared robot instances.
   - If a robot of a specific type already exists, it returns the cached instance instead of creating a new one.

---


### **Sample Output**
```
HUMANOID displayed at coordinates (1, 2)
HUMANOID displayed at coordinates (10, 30)
ROBOTIC_DOG displayed at coordinates (2, 9)
ROBOTIC_DOG displayed at coordinates (11, 19)
humanoidRobot1 == humanoidRobot2: true
roboDog1 == roboDog2: true
```
This confirms that `humanoidRobot1` and `humanoidRobot2` refer to the same object, and the same applies to the robotic dogs, proving that the Flyweight pattern is working.

### **Key Benefits of This Approach**
✅ **Memory Optimization:** Reduces duplicate object creation by sharing common objects.  
✅ **Efficient Performance:** Minimizes object creation overhead by caching instances.  
✅ **Scalability:** Works well in scenarios where millions of objects might need to be created dynamically.

---

<br/><br/><br/><br/>
### Ex-2

It looks like you're implementing the **Flyweight Design Pattern** for a **word processor** to optimize memory usage by sharing character attributes. However, your code has some syntax errors and missing elements. Let me correct and complete it.


```java
import java.util.HashMap;
import java.util.Map;

// Flyweight Interface
interface ILetter {
    void display(int row, int column);
}

// Concrete Flyweight Class
class DocumentCharacter implements ILetter {
    private final char character;
    private final String fontType;
    private final int size;

    public DocumentCharacter(char character, String fontType, int size) {
        this.character = character;
        this.fontType = fontType;
        this.size = size;
    }

    @Override
    public void display(int row, int column) {
        System.out.println("Displaying '" + character + "' at (" + row + ", " + column + ") in font " + fontType + " with size " + size);
    }
}

// Flyweight Factory
class CharacterFactory {
    private static final Map<String, ILetter> characterMap = new HashMap<>();

    public static ILetter getCharacter(char character, String fontType, int size) {
        String key = character + fontType + size;
        if (!characterMap.containsKey(key)) {
            characterMap.put(key, new DocumentCharacter(character, fontType, size));
        }
        return characterMap.get(key);
    }
}

// Client Code
public class Main {
    public static void main(String[] args) {
        String text = "this is a test"; // Example input text
        int row = 0, column = 0;

        for (char ch : text.toCharArray()) {
            if (ch != ' ') { // Ignore spaces
                ILetter letter = CharacterFactory.getCharacter(ch, "Arial", 10);
                letter.display(row, column);
            }
            column++;
        }
    }
}
```

### Explanation:

1. **Flyweight Interface (`ILetter`)**  
   - Defines the `display(int row, int column)` method to render the character.

2. **Concrete Flyweight Class (`DocumentCharacter`)**  
   - Stores intrinsic properties: `character`, `fontType`, and `size`.
   - Implements the `display()` method to show character attributes.

3. **Flyweight Factory (`CharacterFactory`)**  
   - Uses a `HashMap` to store previously created character objects.
   - Returns an existing character object if it already exists, avoiding redundant memory allocation.

4. **Client Code (`Main`)**  
   - Simulates typing text in a word processor.
   - Requests character objects from `CharacterFactory` instead of creating new objects.

### **Advantages of Flyweight Pattern in Word Processor**:
✔ **Memory Optimization** – Reuses objects, reducing memory consumption.  
✔ **Improved Performance** – Faster character rendering in large documents.  
✔ **Efficient Object Creation** – Avoids redundant instantiations.

This implementation ensures that **characters with the same font type and size are shared**, significantly reducing memory usage. 🚀