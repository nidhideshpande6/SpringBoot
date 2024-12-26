# Spring Boot MVC with React Frontend: Detailed Explanation with Example Code

## **Overview**
This guide covers how to integrate **Spring Boot MVC** as the backend with a **React** frontend. We will build a simple application where users can view and add data through a React frontend, and the backend will manage data using Spring Boot.

---

## **Technologies Used**
1. **Spring Boot** for backend development.
2. **React** for frontend development.
3. **Axios** for making API calls.
4. **Thymeleaf** for serving the React app (optional).
5. **Maven** for dependency management.

---

## **Project Structure**
```
springboot-react-app/
|-- backend/
|   |-- src/main/java/com/example/demo/
|   |   |-- controller/
|   |   |   |-- UserController.java
|   |   |-- model/
|   |   |   |-- User.java
|   |   |-- repository/
|   |   |   |-- UserRepository.java
|   |   |-- service/
|   |   |   |-- UserService.java
|   |   |-- DemoApplication.java
|   |-- src/main/resources/
|       |-- static/ (React build files will go here)
|       |-- application.properties
|-- frontend/
|   |-- src/
|       |-- App.js
|       |-- components/
|           |-- UserList.js
|           |-- AddUserForm.js
|   |-- public/
|   |-- package.json
```

---

## **Backend (Spring Boot)**

### 1. **Main Application Class**
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

### 2. **Model**
The `User` class represents the data structure.
```java
package com.example.demo.model;

import jakarta.persistence.*;

@Entity
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String email;

    // Getters and Setters
    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

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

### 3. **Repository**
```java
package com.example.demo.repository;

import com.example.demo.model.User;
import org.springframework.data.jpa.repository.JpaRepository;

public interface UserRepository extends JpaRepository<User, Long> {
}
```

### 4. **Service**
```java
package com.example.demo.service;

import com.example.demo.model.User;
import com.example.demo.repository.UserRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class UserService {

    @Autowired
    private UserRepository userRepository;

    public List<User> getAllUsers() {
        return userRepository.findAll();
    }

    public User saveUser(User user) {
        return userRepository.save(user);
    }
}
```

### 5. **Controller**
```java
package com.example.demo.controller;

import com.example.demo.model.User;
import com.example.demo.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/api/users")
public class UserController {

    @Autowired
    private UserService userService;

    // Get all users
    @GetMapping
    public List<User> getAllUsers() {
        return userService.getAllUsers();
    }

    // Add a new user
    @PostMapping
    public User addUser(@RequestBody User user) {
        return userService.saveUser(user);
    }
}
```

### 6. **Configuration**
In `application.properties`:
```properties
spring.datasource.url=jdbc:mysql://localhost:3306/testdb
spring.datasource.username=root
spring.datasource.password=root
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
```

---

## **Frontend (React)**

### 1. **Set Up React**
Initialize a React app inside the `frontend` folder:
```bash
npx create-react-app frontend
```

Install Axios:
```bash
npm install axios
```

### 2. **App.js**
```javascript
import React from 'react';
import UserList from './components/UserList';
import AddUserForm from './components/AddUserForm';

function App() {
    return (
        <div>
            <h1>User Management</h1>
            <AddUserForm />
            <UserList />
        </div>
    );
}

export default App;
```

### 3. **UserList.js**
```javascript
import React, { useEffect, useState } from 'react';
import axios from 'axios';

const UserList = () => {
    const [users, setUsers] = useState([]);

    useEffect(() => {
        // Fetch users from the backend
        axios.get('/api/users')
            .then(response => {
                setUsers(response.data);
            })
            .catch(error => console.error('Error fetching users:', error));
    }, []);

    return (
        <div>
            <h2>User List</h2>
            <ul>
                {users.map(user => (
                    <li key={user.id}>{user.name} ({user.email})</li>
                ))}
            </ul>
        </div>
    );
};

export default UserList;
```

### 4. **AddUserForm.js**
```javascript
import React, { useState } from 'react';
import axios from 'axios';

const AddUserForm = () => {
    const [name, setName] = useState('');
    const [email, setEmail] = useState('');

    const handleSubmit = (e) => {
        e.preventDefault();

        // Post data to the backend
        axios.post('/api/users', { name, email })
            .then(response => {
                alert('User added successfully!');
                setName('');
                setEmail('');
            })
            .catch(error => console.error('Error adding user:', error));
    };

    return (
        <form onSubmit={handleSubmit}>
            <h2>Add User</h2>
            <div>
                <label>Name:</label>
                <input
                    type="text"
                    value={name}
                    onChange={(e) => setName(e.target.value)}
                    required
                />
            </div>
            <div>
                <label>Email:</label>
                <input
                    type="email"
                    value={email}
                    onChange={(e) => setEmail(e.target.value)}
                    required
                />
            </div>
            <button type="submit">Add User</button>
        </form>
    );
};

export default AddUserForm;
```

---

## **Run the Application**
1. **Backend**:
   ```bash
   mvn spring-boot:run
   ```

2. **Frontend**:
   ```bash
   npm start
   ```

3. Access the application at `http://localhost:3000`. The React app will communicate with the Spring Boot backend.

---

## **Explanation**
1. **Backend**:
   - Spring Boot exposes a REST API at `/api/users` for managing users.
   - Database operations are handled using Spring Data JPA.

2. **Frontend**:
   - React manages the user interface.
   - `Axios` is used to make API requests to the backend.
   - `UserList` displays data fetched from the backend.
   - `AddUserForm` allows adding new users via a POST request.

---

This example demonstrates how to build a complete application using **Spring Boot MVC** and **React**. Let me know if you need further details or enhancements!
