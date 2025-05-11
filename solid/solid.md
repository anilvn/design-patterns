

## 🔷 **Design Patterns Overview**

### 📌 Why Do We Need Design Patterns?

* Design patterns provide **standardized, well-documented solutions** that are widely understood by developers.
* They help solve **common software design problems** using proven techniques, reducing risk and saving development time.
* Design patterns are **language-neutral**, applicable to any object-oriented language.
* Solutions are **easier to understand, maintain, and extend**, improving overall software quality.

---

## 🧩 **Categories of Design Patterns**

Design patterns are grouped into **three main categories**:

1. **Creational Patterns** – Deal with **object creation** mechanisms.
2. **Structural Patterns** – Deal with **object composition** and the relationship between entities.
3. **Behavioral Patterns** – Deal with **object interaction and responsibility** distribution.

---

## 📘 **Creational Design Patterns**

* Focus on **how objects are created** and instantiated.
* Useful when the creation process is complex or should be hidden from the client.

### Examples:

* **Singleton** – Ensures a class has only one instance.
* **Factory Method** – Creates objects without exposing the creation logic to the client.
* **Abstract Factory** – Produces families of related objects (factory of factories).
* **Builder** – Constructs complex objects step by step.
* **Prototype** – Clones existing objects instead of creating new ones.

---

### ❓ **Factory Pattern**

* Used to **encapsulate object creation**.
* Hides the logic of object instantiation and returns objects through a common interface.
* Also known as **Virtual Constructor**.

#### Key Concept:

* Client calls a factory, which returns the required object.
* The client does **not know** how the object is created (e.g., via `new`, autowiring, etc.).

---

### ❓ **Abstract Factory Pattern**

* Also known as **Kit** or **Factory of Factories**.
* Returns a factory, which in turn returns the required object.
* It is **one level higher** than the Factory Pattern.

#### Key Concept:

* Client calls a **factory producer**, which returns a specific factory.
* That factory then returns the desired object.

---

## 📚 **Java/J2EE Design Patterns**

Java/J2EE uses all standard GoF (Gang of Four) patterns and introduces enterprise-level patterns like:

* MVC (Model-View-Controller)
* Front Controller
* DAO (Data Access Object)
* Service Locator
* Business Delegate

---
