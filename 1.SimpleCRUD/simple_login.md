
# User Management Spring Boot Application

This application demonstrates the basic structure of a Spring Boot project with a focus on the Model-View-Controller (MVC) architecture and how to interact with a database using JPA (Java Persistence API).

## Project Structure (MVC)

The Spring Boot application follows the MVC architecture where:

- **Model**: Represents the data structure and business logic.
- **View**: Represents the user interface (typically HTML, but not used directly in this API-based project).
- **Controller**: Handles HTTP requests, processes user inputs, and returns appropriate responses.

### Model Layer

#### User Entity (`User.java`)

This is the data model representing the `User` table in the database.

```java
@Entity
@Table(name = "users")
public class User {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false)
    private String name;

    @Column(nullable = false, unique = true)
    private String email;
    
    // Getters and Setters
}
```

- **`@Entity`**: Marks the class as a JPA entity, which will map to a database table.
- **`@Table(name = "users")`**: Specifies the name of the table in the database that this entity will map to.
- **`@Id`**: Denotes the primary key of the entity.
- **`@GeneratedValue(strategy = GenerationType.IDENTITY)`**: Specifies the strategy for auto-generating primary key values. Here, the value is generated automatically by the database.
- **`@Column(nullable = false)`**: Specifies that this field is non-nullable.
- **`@Column(unique = true)`**: Specifies that the field must have unique values across the table.

#### User Repository (`UserRepo.java`)

```java
import org.springframework.data.jpa.repository.JpaRepository;

public interface UserRepo extends JpaRepository<User, Long> {
    // Custom query methods can be defined here
}
```

- **`JpaRepository<User, Long>`**: Extending `JpaRepository` provides a set of CRUD operations (like `save`, `findAll`, `findById`, `deleteById`, etc.) without needing to implement them manually.
    - **`findById(Long id)`**: Retrieves a `User` entity by its `id`.
    - **`findAll()`**: Retrieves all `User` entities.
    - **`deleteById(Long id)`**: Deletes a `User` entity by its `id`.
- You can also define custom query methods in this interface, for example, `findByEmail(String email)`.

### Controller Layer

#### User Controller (`UserController.java`)

```java
@RestController
@RequestMapping("/users")
public class UserController {

    @Autowired
    private UserRepo userRepository;
    
    @Autowired
    private UserService userService;

    @PostMapping
    public ResponseEntity<String> addUser(@RequestBody User user) {
        userRepository.save(user);
        return ResponseEntity.ok("User added successfully!");
    }
    
    @GetMapping("/{id}")
    public Optional<User> getUserById(@PathVariable Long id) {
        return userRepository.findById(id);
    }
    
    @GetMapping("/all")
    public List<User> getAllUsers() {
        return userRepository.findAll();
    }
    
    @DeleteMapping("/{id}")
    public ResponseEntity<String> deleteUserById(@PathVariable Long id) {
        if (userRepository.existsById(id)) {
            userRepository.deleteById(id);
            return ResponseEntity.ok("User deleted successfully!");
        } else {
            return ResponseEntity.status(404).body("User not found!");
        }
    }
    
    @PutMapping("/{id}/name")
    public ResponseEntity<User> updateName(@PathVariable Long id, @RequestBody Map<String, String> request) {
        String newName = request.get("newName");
        User updatedUser = userService.updateName(id, newName);
        return ResponseEntity.ok(updatedUser);
    }

    @PutMapping("/{id}/email")
    public ResponseEntity<User> updateEmail(@PathVariable Long id, @RequestBody Map<String, String> request) {
        String newEmail = request.get("newEmail");
        User updatedUser = userService.updateEmail(id, newEmail);
        return ResponseEntity.ok(updatedUser);
    }
}
```

- **`@RestController`**: Marks this class as a controller where every method is capable of returning data as JSON (since we are building a RESTful API).
- **`@RequestMapping("/users")`**: Specifies the base URL for all methods in this controller.
- **`@Autowired`**: Used to inject dependencies (`UserRepo` and `UserService`).
- **`@PostMapping`**: Maps HTTP POST requests to `addUser`. This is used for creating a new user.
- **`@GetMapping("/{id}")`**: Maps HTTP GET requests to retrieve a user by ID.
- **`@GetMapping("/all")`**: Maps HTTP GET requests to retrieve all users.
- **`@DeleteMapping("/{id}")`**: Maps HTTP DELETE requests to delete a user by ID.
- **`@PutMapping("/{id}/name")` and `@PutMapping("/{id}/email")`**: Maps HTTP PUT requests to update user information (name or email).

### Service Layer

#### User Service (`UserService.java`)

```java
@Service
public class UserService {

    @Autowired
    private UserRepo userRepository;

    public User updateName(Long id, String newName) {
        User user = userRepository.findById(id).orElseThrow(() -> new RuntimeException("User not found"));
        user.setName(newName);
        return userRepository.save(user);
    }

    public User updateEmail(Long id, String newEmail) {
        User user = userRepository.findById(id).orElseThrow(() -> new RuntimeException("User not found"));
        user.setEmail(newEmail);
        return userRepository.save(user);
    }
}
```

- **`@Service`**: Indicates that this class is a service layer component, which holds business logic and interacts with the repository.
- **`updateName`** and **`updateEmail`**: These methods perform the actual update operations by fetching the user from the database, modifying the entity, and then saving the updated user back to the repository.

### Database Interaction (JPA Repository Methods)

In the `UserRepo` interface, which extends `JpaRepository`, you get several methods automatically:

- **`findById(Long id)`**: Retrieves an entity by its ID. Returns an `Optional<T>` which can either contain the entity or be empty if no entity is found.
- **`findAll()`**: Retrieves all entities of the specified type. Returns a `List<T>`.
- **`save(S entity)`**: Saves the given entity. If the entity already exists (based on the ID), it performs an update; otherwise, it performs an insert.
- **`deleteById(Long id)`**: Deletes an entity by its ID.
- **`existsById(Long id)`**: Checks if an entity exists by its ID.

### Exception Handling

Consider implementing a global exception handler using `@ControllerAdvice` for better error management:

```java
@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(RuntimeException.class)
    public ResponseEntity<String> handleRuntimeException(RuntimeException ex) {
        return ResponseEntity.status(400).body(ex.getMessage());
    }
}
```

This will handle exceptions globally and return appropriate HTTP responses with error messages.

### Conclusion

This Spring Boot application demonstrates a basic user management system following the MVC pattern. The `User` model is persisted to the database using JPA, and CRUD operations are exposed via RESTful endpoints in the `UserController`. Business logic is separated into the `UserService` class. By leveraging `JpaRepository`, basic database operations are performed with minimal code.

### Improvements and Best Practices

- Add **validation** for input data in the controller using `@Valid`.
- Handle **exceptions** more gracefully, using custom exceptions and global exception handlers.
- Consider using **DTOs (Data Transfer Objects)** to avoid directly exposing entity classes to the client.



# application.properties Explanation

The `application.properties` file is used to configure various properties in a Spring Boot application. Below is an explanation of the properties related to data source configuration and server setup:

## DataSource Configuration

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/simplecrud
spring.datasource.username=root
spring.datasource.password=nidhi
```

- **`spring.datasource.url`**: Specifies the URL of the database to which the application will connect. In this case, it is a MySQL database running locally (`localhost`) on port `3306`, and the database name is `simplecrud`.
- **`spring.datasource.username`**: Sets the username used to authenticate the database connection. Here, it is set to `root`.
- **`spring.datasource.password`**: Defines the password used to authenticate the database connection. In this case, it is `nidhi`.

These properties ensure that the Spring Boot application will connect to the MySQL database with the provided credentials.

## Hibernate/JPA Configuration

```properties
spring.jpa.show-sql = true
spring.jpa.hibernate.ddl-auto=update
```

- **`spring.jpa.show-sql`**: When set to `true`, it enables logging of all SQL queries executed by Hibernate (JPA provider) in the console. This can be helpful for debugging and understanding the queries being generated.
- **`spring.jpa.hibernate.ddl-auto`**: This property determines how Hibernate handles the database schema. The `update` value means that Hibernate will automatically update the database schema to reflect any changes made to the JPA entities, such as adding new tables or columns. Other possible values are:
  - `none`: No automatic schema generation or validation.
  - `create`: Drop the schema and recreate it on each application startup (used for development).
  - `validate`: Validates the schema against the entities without making any changes to the database.
  - `update`: Updates the schema if necessary, but does not drop or recreate tables.

## Server Configuration

```properties
server.port=8080
```

- **`server.port`**: Defines the port on which the Spring Boot application will run. By default, it is set to `8080`, meaning the application will be accessible at `http://localhost:8080`.

### Summary

- **DataSource Configuration**: Specifies the database URL, username, and password for connecting to MySQL.
- **JPA/Hibernate Configuration**: Configures Hibernate's behavior in terms of SQL logging and schema updates.
- **Server Configuration**: Defines the port number for the Spring Boot application.

These properties are fundamental for connecting to the database and configuring the application's behavior.

