# Spring `ApplicationContext` and `getBean` Example

## Overview
This project demonstrates how to use `ApplicationContext` and `getBean` in the Spring Framework. It shows how to configure a bean using an XML configuration file and retrieve it in a Java application.

## Steps to Run

1. Clone or download the project.
2. Open the project in your IDE (e.g., IntelliJ IDEA or Eclipse).
3. Ensure Gradle is configured and dependencies are downloaded.
4. Run the `MainApp` class.

## Key Concepts

### 1. ApplicationContext
- `ApplicationContext` is a central interface in the Spring framework that manages the lifecycle and configuration of beans.
- It provides advanced features like event propagation, internationalization, and declarative mechanisms.

### 2. getBean
- `getBean` is a method in `ApplicationContext` used to retrieve beans by name, class type, or both.
- Syntax: 
    - `Object getBean(String name)` 
    - `T getBean(Class<T> requiredType)`.

### 3. Example Code

#### 1. Define a Bean Class
```java
package com.example.demo;

public class HelloWorld {
    private String message;

    public void setMessage(String message) {
        this.message = message;
    }

    public void getMessage() {
        System.out.println("Your Message: " + message);
    }
}
```
2. XML-Based Configuration
xml      
Copy code
<!-- beans.xml -->
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans 
       http://www.springframework.org/schema/beans/spring-beans.xsd">
    
    <!-- Bean Definition -->
    <bean id="helloWorld" class="com.example.demo.HelloWorld">
        <property name="message" value="Hello Spring!" />
    </bean>
</beans>  

3. Main Class Using ApplicationContext and getBean
java
Copy code
```java
package com.example.demo;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MainApp {
    public static void main(String[] args) {
        // Load the Spring configuration
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");

        // Retrieve the bean
        HelloWorld helloWorld = (HelloWorld) context.getBean("helloWorld");

        // Use the bean
        helloWorld.getMessage();
    }
}
 ```

4. Gradle Build File (build.gradle)
groovy
Copy code
```java
plugins {
    id 'java'
}

repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.springframework:spring-context:5.3.29'
}

 

```

Example Output
When you run the program, the following output will be displayed:


Your Message: Hello Spring!
