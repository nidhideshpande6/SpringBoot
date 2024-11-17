# Dependency Injection in Spring

## Overview

**Dependency Injection (DI)** is a design pattern used to achieve **loose coupling** between classes and their dependencies. Rather than a class creating its own dependencies, the dependencies are **injected** into the class by an external source, often a framework like **Spring**. This approach leads to more modular, testable, and maintainable code.

## Why Dependency Injection?

- **Loose Coupling**: Reduces direct dependencies between classes.
- **Easier Testing**: Makes unit testing easier by allowing mock dependencies.
- **Flexible and Scalable**: You can change dependencies without modifying the dependent class.

## How Dependency Injection Works

### 1. Constructor Injection
   Dependencies are passed to a class through its constructor.
   ```java
   public class Car {
       private Engine engine;
   
       public Car(Engine engine) {
           this.engine = engine;
       }
   
       public void start() {
           engine.run();
       }
   }
```
In this case, Car class depends on Engine, and the Engine instance is injected via the constructor.

### 2. Setter Injection
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
Here, the Engine is set using the setEngine() method.

3. Field Injection (Spring-specific)
Dependencies are injected directly into the fields using annotations, commonly used in frameworks like Spring.

java
Copy code
public class Car {
    @Autowired
    private Engine engine;

    public void start() {
        engine.run();
    }
}
Spring will automatically inject an instance of Engine into the Car class at runtime.

Dependency Injection in Spring
Spring Framework implements Dependency Injection through its Inversion of Control (IoC) container. The IoC container manages the creation and injection of dependencies into Spring beans.

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
Constructor Injection: Dependencies are passed via the constructor.
Setter Injection: Dependencies are injected using setter methods.
Interface Injection: The class implements an interface providing a method for injecting dependencies (less common).
Benefits of Dependency Injection
Testability: Allows for easy unit testing by using mock dependencies.
Maintainability: Encourages separation of concerns and reduces the complexity of code.
Flexibility: Easily change or replace dependencies without altering the class.
Reusability: Classes can be reused with different dependencies.
Conclusion
Dependency Injection is a powerful pattern that improves the modularity and maintainability of code by decoupling the creation and management of dependencies. In Spring, DI simplifies object creation and management, making your applications easier to develop and maintain.

Folder Structure (Optional)
If you are following the code example, you may organize the project structure like this:

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
This structure organizes your source code and configuration files for easy navigation and modification.
