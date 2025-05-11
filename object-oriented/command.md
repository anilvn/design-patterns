# Command Pattern

The **Command Design Pattern** is a behavioral design pattern that encapsulates a request as an object, thereby allowing parameterization of clients with different requests, queuing of requests, and logging the request history. It also provides support for undoable operations.

---

## **Example: Remote Control System**
Let's implement a simple **Remote Control System** using the Command Design Pattern in Java.

### **Steps:**
1. Create a `Command` interface.
2. Implement different command classes (`TurnOnCommand`, `TurnOffCommand`).
3. Create a `Light` receiver class.
4. Implement an `Invoker` class (`RemoteControl`).

---

### **1. Command Interface**
```java
// Command Interface
public interface Command {
    void execute();
    void undo();
}
```

---

### **2. Receiver Class (Light)**
```java
// Receiver: Light
public class Light {
    public void turnOn() {
        System.out.println("Light is ON");
    }

    public void turnOff() {
        System.out.println("Light is OFF");
    }
}
```

---

### **3. Concrete Command Classes**
```java
// Concrete Command: TurnOnCommand
public class TurnOnCommand implements Command {
    private Light light;

    public TurnOnCommand(Light light) {
        this.light = light;
    }

    @Override
    public void execute() {
        light.turnOn();
    }

    @Override
    public void undo() {
        light.turnOff();
    }
}

// Concrete Command: TurnOffCommand
public class TurnOffCommand implements Command {
    private Light light;

    public TurnOffCommand(Light light) {
        this.light = light;
    }

    @Override
    public void execute() {
        light.turnOff();
    }

    @Override
    public void undo() {
        light.turnOn();
    }
}
```

---

### **4. Invoker Class (RemoteControl)**
```java
// Invoker: Remote Control
public class RemoteControl {
    private Command command;

    public void setCommand(Command command) {
        this.command = command;
    }

    public void pressButton() {
        command.execute();
    }

    public void pressUndo() {
        command.undo();
    }
}
```

---

### **5. Client Code**
```java
// Client: Main Class
public class CommandPatternDemo {
    public static void main(String[] args) {
        Light light = new Light(); // Receiver

        Command turnOn = new TurnOnCommand(light);
        Command turnOff = new TurnOffCommand(light);

        RemoteControl remote = new RemoteControl();

        // Turn ON the light
        remote.setCommand(turnOn);
        remote.pressButton(); // Output: Light is ON
        remote.pressUndo();   // Output: Light is OFF

        // Turn OFF the light
        remote.setCommand(turnOff);
        remote.pressButton(); // Output: Light is OFF
        remote.pressUndo();   // Output: Light is ON
    }
}
```

---

## **Explanation:**
1. The **Command interface** (`Command`) defines an `execute()` and `undo()` method.
2. The **Concrete Command classes** (`TurnOnCommand`, `TurnOffCommand`) implement `Command` and perform specific actions.
3. The **Receiver class** (`Light`) contains actual business logic.
4. The **Invoker class** (`RemoteControl`) triggers commands and also supports undo operations.
5. The **Client (`CommandPatternDemo`)** initializes and executes commands.

---

## **Advantages of Command Pattern**
- **Decouples sender and receiver:** The client doesn't directly call receiver methods.
- **Supports undo functionality:** Easily revert actions using the `undo()` method.
- **Encapsulates requests as objects:** Can be stored, queued, or logged.




<br/>
<br/>
<br/>
<br/>
<br/>



### **Enhanced Remote Control with Undo Feature**
This implementation improves your existing code and ensures that the **undo** functionality works correctly.

---

### **1. ICommand Interface**
```java
// Command Interface
public interface ICommand {
    void execute();
    void undo();
}
```

---

### **2. Receiver Class (AC)**
```java
// Receiver: AC (Air Conditioner)
public class AC {
    public void turnOn() {
        System.out.println("AC is ON");
    }

    public void turnOff() {
        System.out.println("AC is OFF");
    }
}
```

---

### **3. Concrete Command Classes**
```java
// Concrete Command: TurnOnACCommand
public class TurnOnACCommand implements ICommand {
    private AC ac;

    public TurnOnACCommand(AC ac) {
        this.ac = ac;
    }

    @Override
    public void execute() {
        ac.turnOn();
    }

    @Override
    public void undo() {
        ac.turnOff();
    }
}

// Concrete Command: TurnOffACCommand
public class TurnOffACCommand implements ICommand {
    private AC ac;

    public TurnOffACCommand(AC ac) {
        this.ac = ac;
    }

    @Override
    public void execute() {
        ac.turnOff();
    }

    @Override
    public void undo() {
        ac.turnOn();
    }
}
```

---

### **4. Invoker Class (MyRemoteControl)**
```java
import java.util.Stack;

// Invoker: Remote Control with Undo Feature
public class MyRemoteControl {
    private Stack<ICommand> acCommandHistory = new Stack<>();
    private ICommand command;

    public MyRemoteControl() {}

    public void setCommand(ICommand command) {
        this.command = command;
    }

    public void pressButton() {
        if (command != null) {
            command.execute();
            acCommandHistory.push(command);
        }
    }

    public void undo() {
        if (!acCommandHistory.isEmpty()) {
            ICommand lastCommand = acCommandHistory.pop();
            lastCommand.undo();
        } else {
            System.out.println("No commands to undo.");
        }
    }
}
```

---

### **5. Client Code**
```java
// Client: Test the Remote Control
public class CommandPatternDemo {
    public static void main(String[] args) {
        AC ac = new AC(); // Receiver

        ICommand turnOnAC = new TurnOnACCommand(ac);
        ICommand turnOffAC = new TurnOffACCommand(ac);

        MyRemoteControl remote = new MyRemoteControl();

        // Turning ON AC
        remote.setCommand(turnOnAC);
        remote.pressButton(); // Output: AC is ON

        // Turning OFF AC
        remote.setCommand(turnOffAC);
        remote.pressButton(); // Output: AC is OFF

        // Undo the last operation (AC ON again)
        remote.undo(); // Output: AC is ON

        // Undo the previous operation (AC OFF again)
        remote.undo(); // Output: AC is OFF

        // Undo with empty history
        remote.undo(); // Output: No commands to undo.
    }
}
```

---

### **Key Fixes & Enhancements**
✅ **Fixed Syntax Errors**  
- `acCommandHistory.pop()` now correctly retrieves the last command before calling `undo()`.

✅ **Ensured Stack Operations Work Properly**  
- Used `push()` instead of `add()`, as `Stack` uses `push()` for proper LIFO operations.

✅ **Added Null Check for Commands**  
- Prevents `NullPointerException` if `setCommand()` is not called before `pressButton()`.

✅ **Added Empty Stack Check in `undo()`**  
- Displays `"No commands to undo."` if undo is attempted with an empty history.

---

### **Output**
```
AC is ON
AC is OFF
AC is ON
AC is OFF
No commands to undo.
```
