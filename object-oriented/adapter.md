# The Adapter Design Pattern

> **The Adapter Design Pattern** is a structural design pattern that allows objects with incompatible interfaces to work together.  
> It acts as a bridge between two different interfaces by wrapping one of the objects and translating its interface into a form the client expects.

---

### In simple words:
- **Adapter** makes two incompatible classes **compatible** without changing their code.
- It **converts** the interface of a class into another interface that a client wants.

---

### **Example in your context:**
- `WeightMachine` gives weight in **pounds**.
- But your client (main program) needs **kilograms**.
- So you created a **WeightMachineAdapter** that **adapts** `WeightMachine`'s output (pounds) to what the client wants (kilograms).

---

### **Real-life examples:**
- **Mobile Charger Adapter**: Mobile needs 5V, but the power socket gives 240V.  
  The **charger adapter** converts 240V into 5V.
- **Memory Card Reader**: A laptop without a memory card slot can use an adapter to read the memory card via a USB port.

---

### Ex-1

### **1. Remove Unnecessary Package Imports**
You have `import LowLevelDesign.DesignPatterns.AdapterDesignPattern.Adaptee.WeightMachine;` and other imports that might not be necessary if your classes are in the same package.

### **2. Use Constants for Conversion**
Instead of hardcoding `0.45`, define a constant for clarity and easy modifications in the future.

### **3. Improve Readability**
Instead of storing the intermediate `weightInKg`, return the calculated value directly.

---

### **Improved Code**
#### **WeightMachine.java (Interface)**
```java
public interface WeightMachine {
    // Returns the weight in Pounds
    double getWeightInPound();
}
```

#### **WeightMachineForBabies.java (Adaptee)**
```java
public class WeightMachineForBabies implements WeightMachine {
    @Override
    public double getWeightInPound() {
        return 28;  // Example weight
    }
}
```

#### **WeightMachineAdapter.java (Adapter Interface)**
```java
public interface WeightMachineAdapter {
    double getWeightInKg();
}
```

#### **WeightMachineAdapterImpl.java (Adapter Implementation)**
```java
public class WeightMachineAdapterImpl implements WeightMachineAdapter {

    private static final double POUND_TO_KG_CONVERSION = 0.45;
    private WeightMachine weightMachine;

    public WeightMachineAdapterImpl(WeightMachine weightMachine) {
        this.weightMachine = weightMachine;
    }

    @Override
    public double getWeightInKg() {
        return weightMachine.getWeightInPound() * POUND_TO_KG_CONVERSION;
    }
}
```

#### **Main.java (Client Code)**
```java
public class Main {
    public static void main(String[] args) {
        WeightMachineAdapter weightMachineAdapter = new WeightMachineAdapterImpl(new WeightMachineForBabies());
        System.out.println("Weight in KG: " + weightMachineAdapter.getWeightInKg());
    }
}
```

---

### **Expected Output:**
```
Weight in KG: 12.6
```

### **Key Improvements:**
✅ Removed unnecessary imports  
✅ Used a **constant** for conversion (`POUND_TO_KG_CONVERSION`)  
✅ Improved **readability** with direct return  
✅ **Encapsulation**: Made `weightMachine` `private`  


