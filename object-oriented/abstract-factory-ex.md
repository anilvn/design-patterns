# Java Design Patterns: Singleton, Abstract Factory, Builder, and Prototype

## Table of Contents

| # | Question |
|---|----------|
| **Abstract Factory Pattern** |
| 1 | [What is the Abstract Factory Design Pattern?](#what-is-the-abstract-factory-design-pattern) |
| 2 | [How to implement Abstract Factory Pattern in Java?](#how-to-implement-abstract-factory-pattern-in-java) |
| 3 | [What are the advantages and disadvantages of Abstract Factory Pattern?](#what-are-the-advantages-and-disadvantages-of-abstract-factory-pattern) |
| 4 | [When should you use Abstract Factory vs Factory Method patterns?](#when-should-you-use-abstract-factory-vs-factory-method-patterns) |



## What is the Abstract Factory Design Pattern?

The Abstract Factory pattern is a creational design pattern that provides an interface for creating families of related or dependent objects without specifying their concrete classes.

Key features:
- **Factory of Factories**: Creates related objects without specifying concrete classes
- **Related Products**: Groups related products into families
- **Consistency**: Ensures all products in a family work together
- **Abstraction Layer**: Isolates concrete classes from client code

```java
// Abstract product interfaces
interface Button {
    void render();
    void onClick();
}

interface Checkbox {
    void render();
    void toggle();
}

// Concrete products for "Light" theme
class LightButton implements Button {
    @Override
    public void render() {
        System.out.println("Rendering a light button");
    }
    
    @Override
    public void onClick() {
        System.out.println("Light button clicked");
    }
}

class LightCheckbox implements Checkbox {
    @Override
    public void render() {
        System.out.println("Rendering a light checkbox");
    }
    
    @Override
    public void toggle() {
        System.out.println("Light checkbox toggled");
    }
}

// Concrete products for "Dark" theme
class DarkButton implements Button {
    @Override
    public void render() {
        System.out.println("Rendering a dark button");
    }
    
    @Override
    public void onClick() {
        System.out.println("Dark button clicked");
    }
}

class DarkCheckbox implements Checkbox {
    @Override
    public void render() {
        System.out.println("Rendering a dark checkbox");
    }
    
    @Override
    public void toggle() {
        System.out.println("Dark checkbox toggled");
    }
}

// Abstract factory interface
interface GUIFactory {
    Button createButton();
    Checkbox createCheckbox();
}

// Concrete factories
class LightThemeFactory implements GUIFactory {
    @Override
    public Button createButton() {
        return new LightButton();
    }
    
    @Override
    public Checkbox createCheckbox() {
        return new LightCheckbox();
    }
}

class DarkThemeFactory implements GUIFactory {
    @Override
    public Button createButton() {
        return new DarkButton();
    }
    
    @Override
    public Checkbox createCheckbox() {
        return new DarkCheckbox();
    }
}
```

Usage example:
```java
public class Application {
    private Button button;
    private Checkbox checkbox;
    
    public Application(GUIFactory factory) {
        button = factory.createButton();
        checkbox = factory.createCheckbox();
    }
    
    public void render() {
        button.render();
        checkbox.render();
    }
    
    public static void main(String[] args) {
        // Create application with light theme
        GUIFactory lightFactory = new LightThemeFactory();
        Application lightApp = new Application(lightFactory);
        lightApp.render();
        
        // Create application with dark theme
        GUIFactory darkFactory = new DarkThemeFactory();
        Application darkApp = new Application(darkFactory);
        darkApp.render();
        
        // Output:
        // Rendering a light button
        // Rendering a light checkbox
        // Rendering a dark button
        // Rendering a dark checkbox
    }
}
```

[Back to Top](#table-of-contents)

## How to implement Abstract Factory Pattern in Java?

Implementing Abstract Factory pattern in Java involves creating abstract product interfaces, concrete product classes, an abstract factory interface, and concrete factory implementations.

Step-by-step implementation:

1. **Define abstract product interfaces** for each type of product
2. **Create concrete product classes** implementing these interfaces
3. **Define an abstract factory interface** with methods to create each product
4. **Implement concrete factory classes** for each product family
5. **Use the factory** in client code

```java
// STEP 1: Abstract Product A - Database Connection
interface DbConnection {
    void connect();
    void executeQuery(String query);
    void close();
}

// STEP 1: Abstract Product B - REST Client
interface RestClient {
    void init();
    void makeRequest(String endpoint);
    void processResponse();
}

// STEP 2: Concrete Products for MySQL family
class MySQLConnection implements DbConnection {
    @Override
    public void connect() {
        System.out.println("Connecting to MySQL database");
    }
    
    @Override
    public void executeQuery(String query) {
        System.out.println("Executing MySQL query: " + query);
    }
    
    @Override
    public void close() {
        System.out.println("Closing MySQL connection");
    }
}

class MySQLRestClient implements RestClient {
    @Override
    public void init() {
        System.out.println("Initializing MySQL REST client");
    }
    
    @Override
    public void makeRequest(String endpoint) {
        System.out.println("Making MySQL REST request to: " + endpoint);
    }
    
    @Override
    public void processResponse() {
        System.out.println("Processing MySQL REST response");
    }
}

// STEP 2: Concrete Products for Oracle family
class OracleConnection implements DbConnection {
    @Override
    public void connect() {
        System.out.println("Connecting to Oracle database");
    }
    
    @Override
    public void executeQuery(String query) {
        System.out.println("Executing Oracle query: " + query);
    }
    
    @Override
    public void close() {
        System.out.println("Closing Oracle connection");
    }
}

class OracleRestClient implements RestClient {
    @Override
    public void init() {
        System.out.println("Initializing Oracle REST client");
    }
    
    @Override
    public void makeRequest(String endpoint) {
        System.out.println("Making Oracle REST request to: " + endpoint);
    }
    
    @Override
    public void processResponse() {
        System.out.println("Processing Oracle REST response");
    }
}

// STEP 3: Abstract Factory
interface DataSourceFactory {
    DbConnection createDbConnection();
    RestClient createRestClient();
}

// STEP 4: Concrete Factories
class MySQLDataSourceFactory implements DataSourceFactory {
    @Override
    public DbConnection createDbConnection() {
        return new MySQLConnection();
    }
    
    @Override
    public RestClient createRestClient() {
        return new MySQLRestClient();
    }
}

class OracleDataSourceFactory implements DataSourceFactory {
    @Override
    public DbConnection createDbConnection() {
        return new OracleConnection();
    }
    
    @Override
    public RestClient createRestClient() {
        return new OracleRestClient();
    }
}
```

Usage example (STEP 5):
```java
public class DataProcessor {
    private final DbConnection dbConnection;
    private final RestClient restClient;
    
    // Client receives factory as parameter
    public DataProcessor(DataSourceFactory factory) {
        // Create products using the factory
        this.dbConnection = factory.createDbConnection();
        this.restClient = factory.createRestClient();
    }
    
    public void processData() {
        dbConnection.connect();
        dbConnection.executeQuery("SELECT * FROM data");
        
        restClient.init();
        restClient.makeRequest("/api/data");
        restClient.processResponse();
        
        dbConnection.close();
    }
    
    public static void main(String[] args) {
        // Configure with MySQL factory
        DataSourceFactory mySqlFactory = new MySQLDataSourceFactory();
        DataProcessor mySqlProcessor = new DataProcessor(mySqlFactory);
        System.out.println("Processing with MySQL:");
        mySqlProcessor.processData();
        
        System.out.println("\nProcessing with Oracle:");
        // Switch to Oracle factory
        DataSourceFactory oracleFactory = new OracleDataSourceFactory();
        DataProcessor oracleProcessor = new DataProcessor(oracleFactory);
        oracleProcessor.processData();
        
        // Output:
        // Processing with MySQL:
        // Connecting to MySQL database
        // Executing MySQL query: SELECT * FROM data
        // Initializing MySQL REST client
        // Making MySQL REST request to: /api/data
        // Processing MySQL REST response
        // Closing MySQL connection
        //
        // Processing with Oracle:
        // Connecting to Oracle database
        // Executing Oracle query: SELECT * FROM data
        // Initializing Oracle REST client
        // Making Oracle REST request to: /api/data
        // Processing Oracle REST response
        // Closing Oracle connection
    }
}
```

[Back to Top](#table-of-contents)

## What are the advantages and disadvantages of Abstract Factory Pattern?

### Advantages

- **Isolation**: Client code is isolated from concrete class implementations
- **Family Consistency**: Ensures products from the same family work together
- **Product Exchange**: Easy to exchange product families
- **Single Responsibility**: Each factory encapsulates the creation of a product family
- **Open/Closed Principle**: Adding new products doesn't break existing code

### Disadvantages

- **Complexity**: Adds complexity with many interfaces and classes
- **Tight Factory Coupling**: Products are tied to their specific factory
- **Difficult Extension**: Adding new types of products requires modifying all factories
- **Abstraction Overhead**: May introduce unnecessary abstraction for simple cases
- **Design Decisions**: Product family must be decided early

Comparison with other patterns:

| Aspect | Abstract Factory | Factory Method | Builder |
|--------|-----------------|---------------|---------|
| **Purpose** | Creates families of related objects | Creates a single object | Constructs complex objects step by step |
| **Complexity** | Higher | Medium | Medium |
| **Flexibility** | Family-based | Single product | Single product with many configurations |
| **Use Case** | Multiple related product families | Single product with subtypes | Objects with many optional parameters |
| **Structure** | Multiple factories and products | Single factory with product hierarchy | Single builder for complex object |

[Back to Top](#table-of-contents)

## When should you use Abstract Factory vs Factory Method patterns?

The choice between Abstract Factory and Factory Method depends on your specific design requirements.

### Use Abstract Factory when:

- **Related Product Families**: You need to create families of related objects
- **Multiple Product Variants**: You have multiple variations of several products
- **Product Consistency**: Products must work together consistently
- **Abstraction from Concrete Classes**: You want to isolate client code from concrete classes
- **Platform Independence**: You need platform-specific implementations of interfaces

### Use Factory Method when:

- **Single Product Type**: You're creating variations of a single product
- **Subclass Extensibility**: You want subclasses to specify which objects to create
- **Delegation**: You want to delegate object creation to subclasses
- **Reusable Framework**: You're building a framework with extension points
- **Unknown Future Types**: You don't know all concrete types at design time

Code comparison:

```java
// Factory Method pattern example
abstract class DocumentCreator {
    // Factory Method
    protected abstract Document createDocument();
    
    // Template method that uses the factory method
    public final void openDocument() {
        Document doc = createDocument();
        doc.open();
    }
}

class PDFCreator extends DocumentCreator {
    @Override
    protected Document createDocument() {
        return new PDFDocument();
    }
}

class WordCreator extends DocumentCreator {
    @Override
    protected Document createDocument() {
        return new WordDocument();
    }
}

// Usage
DocumentCreator creator = new PDFCreator();
creator.openDocument(); // Creates and opens a PDF
```

```java
// Abstract Factory pattern example (simplified from previous examples)
interface GUIFactory {
    Button createButton();
    Checkbox createCheckbox();
}

class WindowsFactory implements GUIFactory {
    @Override
    public Button createButton() {
        return new WindowsButton();
    }
    
    @Override
    public Checkbox createCheckbox() {
        return new WindowsCheckbox();
    }
}

class MacFactory implements GUIFactory {
    @Override
    public Button createButton() {
        return new MacButton();
    }
    
    @Override
    public Checkbox createCheckbox() {
        return new MacCheckbox();
    }
}

// Usage
GUIFactory factory = new WindowsFactory();
Button button = factory.createButton();
Checkbox checkbox = factory.createCheckbox();
```

Decision Matrix:

| Criteria | Factory Method | Abstract Factory |
|----------|---------------|-----------------|
| **Number of Products** | Single product hierarchy | Multiple product hierarchies |
| **Number of Variations** | Multiple implementations of one product | Multiple implementations of multiple products |
| **Relationship Between Products** | Independent | Related/dependent |
| **Complexity** | Lower | Higher |
| **Extension Method** | Subclassing | Implementing new factories |


[Back to Top](#table-of-contents)


