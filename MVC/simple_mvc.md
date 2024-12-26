# Spring Boot MVC Notes

## **Overview of Spring Boot MVC**
Spring Boot MVC is a framework that simplifies the development of web applications using the Model-View-Controller (MVC) design pattern. It is built on top of the Spring MVC framework and provides default configurations, reducing the need for manual setup.

---

## **Core Concepts**

### **1. DispatcherServlet**
- Central to Spring MVC architecture.
- Handles incoming HTTP requests and delegates them to the appropriate handlers (controllers).

### **2. Model-View-Controller (MVC)**
- **Model**: Represents the application data.
- **View**: Renders the data (HTML, Thymeleaf, etc.).
- **Controller**: Handles user input, interacts with the model, and selects the view to render.

### **3. Auto-Configuration**
- Spring Boot provides auto-configuration for commonly used components like DispatcherServlet, DataSources, ViewResolvers, etc.

---

## **Setting Up a Spring Boot MVC Project**

### **1. Dependencies**
Add the following dependencies to your `pom.xml`:

```xml
<dependencies>
    <!-- Spring Boot Starter Web for MVC -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!-- Thymeleaf for Views (optional) -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-thymeleaf</artifactId>
    </dependency>

    <!-- Spring Boot Starter Test for Testing -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

---

### **2. Application Structure**
A typical Spring Boot MVC application follows this structure:

```
src/main/java
|-- com.example.demo
    |-- controller
    |   |-- HomeController.java
    |-- model
    |   |-- User.java
    |-- service
    |   |-- UserService.java
    |-- DemoApplication.java

src/main/resources
|-- templates
|   |-- index.html
|-- application.properties
```

---

## **Creating a Basic Spring Boot MVC Application**

### **Step 1: Create the Main Application Class**
```java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

---

### **Step 2: Create a Controller**
```java
package com.example.demo.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class HomeController {

    @GetMapping("/")
    public String home(Model model) {
        model.addAttribute("message", "Welcome to Spring Boot MVC!");
        return "index"; // Points to index.html in the templates folder
    }
}
```

---

### **Step 3: Create a View (Thymeleaf)**
In `src/main/resources/templates/index.html`:

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Spring Boot MVC</title>
</head>
<body>
    <h1 th:text="${message}"></h1>
</body>
</html>
```

---

### **Step 4: Run the Application**
1. Start the application using `DemoApplication`.
2. Open a browser and navigate to `http://localhost:8080`.
3. You should see the message: **Welcome to Spring Boot MVC!**

---

## **Advanced Topics**

### **1. Handling Form Data**
#### Controller:
```java
package com.example.demo.controller;

import com.example.demo.model.User;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.*;

@Controller
public class UserController {

    @GetMapping("/register")
    public String showForm(Model model) {
        model.addAttribute("user", new User());
        return "register";
    }

    @PostMapping("/register")
    public String submitForm(@ModelAttribute User user, Model model) {
        model.addAttribute("message", "User registered successfully!");
        return "success";
    }
}
```

#### Model:
```java
package com.example.demo.model;

public class User {
    private String name;
    private String email;

    // Getters and Setters
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }
}
```

#### Views:
1. `register.html`
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Register</title>
</head>
<body>
    <form action="/register" method="post">
        <label for="name">Name:</label>
        <input type="text" id="name" name="name"><br>

        <label for="email">Email:</label>
        <input type="email" id="email" name="email"><br>

        <button type="submit">Submit</button>
    </form>
</body>
</html>
```

2. `success.html`
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Success</title>
</head>
<body>
    <h1 th:text="${message}"></h1>
</body>
</html>
```

---

### **2. REST API with Spring Boot MVC**
#### Controller:
```java
package com.example.demo.controller;

import org.springframework.web.bind.annotation.*;
import java.util.List;

@RestController
@RequestMapping("/api")
public class ApiController {

    @GetMapping("/users")
    public List<String> getUsers() {
        return List.of("User1", "User2", "User3");
    }

    @PostMapping("/users")
    public String addUser(@RequestBody String user) {
        return "User added: " + user;
    }
}
```

---

## **Best Practices**
1. Keep the structure modular with packages for controllers, models, and services.
2. Use `application.properties` or `application.yml` for environment-specific configurations.
3. Leverage Spring Boot DevTools for auto-reload during development.
4. Follow RESTful principles when designing APIs.
5. Write unit tests for controllers and services.

---

## **Common Annotations**
- `@Controller`: Marks a class as a controller in the MVC architecture.
- `@RestController`: Combines `@Controller` and `@ResponseBody` for REST APIs.
- `@GetMapping`, `@PostMapping`, `@RequestMapping`: Map HTTP requests to handler methods.
- `@ModelAttribute`: Binds form data to model objects.
- `@RequestBody`: Binds the body of the HTTP request to a method parameter.
- `@ResponseBody`: Indicates that the return value should be written directly to the HTTP response.

---

## **Conclusion**
Spring Boot MVC simplifies web application development by reducing configuration and setup time. With powerful features and integrations, it is a go-to choice for creating modern web applications.
