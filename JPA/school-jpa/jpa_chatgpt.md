# Spring JPA Notes

Spring Data JPA simplifies the data access layer by providing a repository abstraction over JPA (Java Persistence API). Here's a detailed overview of the essential concepts, repositories, annotations, and their usage.

---

## 1. **Introduction to JPA**
JPA (Java Persistence API) is a specification for managing relational data in Java applications. Spring Data JPA builds on JPA and simplifies database interactions.

### Key Features:
- Simplifies ORM (Object-Relational Mapping)
- Reduces boilerplate code
- Offers pagination and sorting
- Supports query methods and JPQL (Java Persistence Query Language)

---

## 2. **Spring JPA Dependencies**
Add the required dependencies in your `pom.xml` (Maven):

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <scope>runtime</scope>
</dependency>
```

---

## 3. **Configuration**

### `application.properties` (Example for H2 Database):
```properties
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driver-class-name=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=
spring.jpa.hibernate.ddl-auto=update
spring.h2.console.enabled=true
```

---

## 4. **Entity Class**
An entity represents a table in the database. Annotate it with `@Entity`.

### Example:
```java
import jakarta.persistence.*;

@Entity
@Table(name = "users")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false)
    private String name;

    @Column(unique = true)
    private String email;

    // Getters and Setters
}
```

### Key Annotations in Entities:
| Annotation                  | Description                                  |
|-----------------------------|----------------------------------------------|
| `@Entity`                   | Marks a class as a JPA entity.              |
| `@Table`                    | Specifies the table name.                   |
| `@Id`                       | Marks the primary key field.                |
| `@GeneratedValue`           | Defines ID generation strategy.             |
| `@Column`                   | Configures column properties.               |
| `@OneToOne`, `@OneToMany`   | Defines relationships between entities.      |
| `@ManyToOne`, `@ManyToMany` | Defines relationships between entities.      |

---

## 5. **Repository Layer**
Repositories abstract the data access layer. Spring Data JPA provides several repository interfaces.

### Common Repository Interfaces:
- `CrudRepository<T, ID>`: Basic CRUD operations
- `JpaRepository<T, ID>`: Extends `CrudRepository` with JPA-specific methods
- `PagingAndSortingRepository<T, ID>`: Adds support for pagination and sorting

### Example:
```java
import org.springframework.data.jpa.repository.JpaRepository;

public interface UserRepository extends JpaRepository<User, Long> {
    // Custom query methods
    User findByEmail(String email);
}
```

### Derived Query Methods:
Spring JPA can derive queries based on method names.

#### Examples:
| Method Name               | Query                                   |
|---------------------------|-----------------------------------------|
| `findByName(String name)` | `SELECT u FROM User u WHERE u.name = ?1`|
| `findByEmailAndName()`    | `SELECT u FROM User u WHERE u.email = ?1 AND u.name = ?2`|

---

## 6. **JPQL and Native Queries**

### JPQL:
JPQL queries use entity class names and properties instead of table names.

```java
@Query("SELECT u FROM User u WHERE u.name = :name")
User findUserByName(@Param("name") String name);
```

### Native Query:
Allows execution of raw SQL queries.

```java
@Query(value = "SELECT * FROM users WHERE email = :email", nativeQuery = true)
User findByEmailNative(@Param("email") String email);
```

---

## 7. **Transactional Management**
Spring provides `@Transactional` for managing transactions.

### Example:
```java
@Transactional
public void updateUser(Long id, String name) {
    User user = userRepository.findById(id).orElseThrow(() -> new RuntimeException("User not found"));
    user.setName(name);
    userRepository.save(user);
}
```

### Propagation Types:
| Type             | Description                                      |
|------------------|--------------------------------------------------|
| `REQUIRED`       | Joins existing transaction, or creates one.      |
| `REQUIRES_NEW`   | Suspends existing transaction and creates new.   |
| `NESTED`         | Executes within a nested transaction.            |

---

## 8. **Pagination and Sorting**

### Pagination:
```java
Page<User> findAll(Pageable pageable);
```
Usage:
```java
Pageable pageable = PageRequest.of(0, 10, Sort.by("name"));
Page<User> page = userRepository.findAll(pageable);
```

### Sorting:
```java
List<User> findAll(Sort sort);
```
Usage:
```java
List<User> users = userRepository.findAll(Sort.by("name").descending());
```

---

## 9. **Entity Relationships**

### Example:
#### One-to-Many:
```java
@Entity
public class Department {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @OneToMany(mappedBy = "department")
    private List<Employee> employees;
}

@Entity
public class Employee {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ManyToOne
    private Department department;
}
```

#### Many-to-Many:
```java
@Entity
public class Student {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ManyToMany
    @JoinTable(
        name = "student_course",
        joinColumns = @JoinColumn(name = "student_id"),
        inverseJoinColumns = @JoinColumn(name = "course_id")
    )
    private List<Course> courses;
}

@Entity
public class Course {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ManyToMany(mappedBy = "courses")
    private List<Student> students;
}
```

---

## 10. **Auditing**
Enable auditing to automatically populate created/updated timestamps.

### Configuration:
```java
@Configuration
@EnableJpaAuditing
public class JpaConfig {}
```

### Example:
```java
@MappedSuperclass
@EntityListeners(AuditingEntityListener.class)
public abstract class Auditable {

    @CreatedDate
    @Column(updatable = false)
    private LocalDateTime createdDate;

    @LastModifiedDate
    private LocalDateTime lastModifiedDate;
}

@Entity
public class Product extends Auditable {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
}
```

---

## 11. **Best Practices**
- Use DTOs for data transfer to avoid exposing entity structures.
- Always use pagination for large datasets.
- Optimize queries using indexes in the database.
- Use `@Transactional` judiciously to manage transactions efficiently.

---

This guide covers the essential concepts of Spring Data JPA. For more advanced usage, explore Spring documentation and practice building applications.
