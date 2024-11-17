Dependency Injection in Spring
Overview
Dependency Injection (DI) is a design pattern that achieves loose coupling between classes and their dependencies. 
Instead of a class creating its own dependencies, the dependencies are injected into the class from an external source. 
This pattern improves modularity, testability, and maintainability.

Why Dependency Injection?
Loose Coupling: Reduces dependency between classes.
Easier Testing: Allows easier unit testing with mock dependencies.
Flexible and Scalable: Dependencies can be easily changed without altering the dependent class.

How Dependency Injection Works
1. Constructor Injection
Dependencies are passed to a class through its constructor.

java
Copy code
public class Car {
    private Engine engine;

    public Car(Engine engine) {
        this.engine = engine;
    }

    public void start() {
        engine.run();
    }
}

Here, the Car class depends on the Engine class, and the Engine is passed via the constructor.

2. Setter Injection
Dependencies are provided through setter methods.

java
Copy code
public class Car {
    private Engine engine;

    public void setEngine(Engine engine) {
        this.engine = engine;
    }

    public void start() {
        engine.run();
    }
}
The Engine is set using the setEngine() method.

3. Field Injection (Spring-specific)
Dependencies are injected directly into the fields using annotations (commonly used in frameworks like Spring).

java
Copy code
public class Car {
    @Autowired
    private Engine engine;

    public void start() {
        engine.run();
    }
}
Spring will automatically inject an instance of the Engine class into the Car class at runtime.

Dependency Injection in Spring
In Spring, DI is implemented using the Inversion of Control (IoC) container. Spring automatically injects the dependencies into the objects (beans) it manages.

Spring DI Example:
Engine Class (Dependency)

java
Copy code
@Component
public class Engine {
    public void start() {
        System.out.println("Engine started.");
    }
}
Car Class (Dependent)

java
Copy code
@Component
public class Car {
    private Engine engine;

    @Autowired
    public Car(Engine engine) {
        this.engine = engine;
    }

    public void start() {
        engine.start();
        System.out.println("Car is running.");
    }
}
Spring Configuration (AppConfig)

java
Copy code
@Configuration
@ComponentScan(basePackages = "com.example")
public class AppConfig {
}
Main Application

java
Copy code
public class Main {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext context = 
            new AnnotationConfigApplicationContext(AppConfig.class);
        
        Car car = context.getBean(Car.class);
        car.start(); // The Car will have its Engine injected and started.
    }
}
Types of Dependency Injection
Constructor Injection: Dependencies are passed via the class constructor.
Setter Injection: Dependencies are injected through setter methods.
Interface Injection: The class implements an interface that provides the method to inject dependencies (less common).
Benefits of Dependency Injection
Testability: Makes unit testing easier by allowing mock dependencies.
Maintainability: Reduces complexity and promotes separation of concerns.
Flexibility: You can easily replace or change dependencies.
Reusability: Classes can be reused with different dependencies.
Conclusion
Dependency Injection is a powerful design pattern that helps achieve cleaner, more modular code. In Spring, DI simplifies object creation and management, leading to more maintainable and testable applications.

Folder Structure (optional)
If you want to demonstrate this project in your GitHub repo, you can create the following structure:

bash
Copy code
/SpringDI
│
├── /src
│   ├── /main
│   │   └── /java
│   │       └── /com
│   │           └── /example
│   │               ├── Car.java
│   │               ├── Engine.java
│   │               └── AppConfig.java
│   │
│   └── /resources
│       └── application.properties
│
└── pom.xml (for Maven projects)
