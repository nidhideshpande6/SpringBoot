# JPA: Inheritance and Composition

## 1. **Inheritance in JPA**

### Definition
- JPA provides strategies to map class inheritance hierarchies to database tables.
- Inheritance can be implemented using the `@Inheritance` annotation.

### Strategies

#### **1.1 Single Table Strategy**
- All entities in the hierarchy are mapped to a single table.
- Discriminator column differentiates entity types.

##### Code Example
```java
import javax.persistence.*;

@Entity
@Inheritance(strategy = InheritanceType.SINGLE_TABLE)
@DiscriminatorColumn(name = "entity_type", discriminatorType = DiscriminatorType.STRING)
public abstract class Person {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    // Getters and setters
}

@Entity
@DiscriminatorValue("Student")
public class Student extends Person {
    private String course;

    // Getters and setters
}

@Entity
@DiscriminatorValue("Teacher")
public class Teacher extends Person {
    private String subject;

    // Getters and setters
}
```

##### Schema
| ID  | NAME      | ENTITY_TYPE | COURSE   | SUBJECT |
|-----|-----------|-------------|----------|---------|
| 1   | Alice     | Student     | Math     | NULL    |
| 2   | Bob       | Teacher     | NULL     | Science |

#### **1.2 Joined Strategy**
- Each entity is mapped to a separate table.
- Subclass tables share the primary key with the parent table.

##### Code Example
```java
@Entity
@Inheritance(strategy = InheritanceType.JOINED)
public abstract class Person {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    // Getters and setters
}

@Entity
public class Student extends Person {
    private String course;

    // Getters and setters
}

@Entity
public class Teacher extends Person {
    private String subject;

    // Getters and setters
}
```

##### Schema
**Person Table**
| ID  | NAME      |
|-----|-----------|
| 1   | Alice     |
| 2   | Bob       |

**Student Table**
| ID  | COURSE   |
|-----|----------|
| 1   | Math     |

**Teacher Table**
| ID  | SUBJECT  |
|-----|----------|
| 2   | Science  |

#### **1.3 Table Per Class Strategy**
- Each entity is mapped to a separate table with all fields from the parent class.

##### Code Example
```java
@Entity
@Inheritance(strategy = InheritanceType.TABLE_PER_CLASS)
public abstract class Person {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    // Getters and setters
}

@Entity
public class Student extends Person {
    private String course;

    // Getters and setters
}

@Entity
public class Teacher extends Person {
    private String subject;

    // Getters and setters
}
```

##### Schema
**Student Table**
| ID  | NAME      | COURSE   |
|-----|-----------|----------|
| 1   | Alice     | Math     |

**Teacher Table**
| ID  | NAME      | SUBJECT  |
|-----|-----------|----------|
| 2   | Bob       | Science  |

---

## 2. **Composition in JPA**

### Definition
- Composition involves embedding one entity or value object into another entity.
- Implemented using `@Embeddable` and `@Embedded` annotations.

### Code Example
```java
import javax.persistence.*;

@Embeddable
public class Address {
    private String street;
    private String city;

    // Getters and setters
}

@Entity
public class Employee {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    @Embedded
    private Address address;

    // Getters and setters
}
```

### Schema
| ID  | NAME      | STREET      | CITY       |
|-----|-----------|-------------|------------|
| 1   | John Doe  | Elm Street  | Gotham     |

---

## 3. **Annotations Overview**

### `@Inheritance`
Defines inheritance mapping strategy for an entity hierarchy.

### `@DiscriminatorColumn`
Defines a column to differentiate entity types in `SINGLE_TABLE` strategy.

### `@DiscriminatorValue`
Specifies the value used in the discriminator column for a specific entity.

### `@Embeddable`
Marks a class as embeddable in another entity.

### `@Embedded`
Indicates an embeddable class is used as a field in an entity.

### `@MappedSuperclass`
Allows sharing mappings between entities without making the class an entity.

---

## 4. **Inheritance vs. Composition**

### Comparison
| Feature             | Inheritance                                   | Composition                                   |
|---------------------|-----------------------------------------------|---------------------------------------------|
| **Purpose**         | Models "is-a" relationships.                 | Models "has-a" relationships.               |
| **Schema Design**   | Involves multiple tables (depending on strategy). | Single table with embedded fields.         |
| **Flexibility**     | Harder to change hierarchy.                  | Easier to modify and refactor.              |
| **Use Case**        | Suitable for entities sharing common behavior.| Suitable for entities with reusable fields. |

### Which is Better?
- **Inheritance**: Use when you have a clear hierarchy with common behavior.
- **Composition**: Use when you want to reuse fields without creating a strict hierarchy.

---

## 5. **JPA vs Hibernate in Inheritance and Composition**

| Feature                  | JPA                                      | Hibernate                                  |
|--------------------------|------------------------------------------|-------------------------------------------|
| **Inheritance Support**  | Fully supports `SINGLE_TABLE`, `JOINED`, and `TABLE_PER_CLASS`. | Same as JPA with additional annotations like `@DiscriminatorFormula`. |
| **Composition Support**  | Fully supports `@Embeddable` and `@Embedded`. | Same as JPA with extra features for custom embedding. |
| **Custom Features**      | Relies on standard annotations.          | Provides advanced caching and dynamic insert/update options. |

---

## 6. **Conclusion**
- JPA provides robust support for both inheritance and composition through standard annotations.
- Hibernate extends these capabilities with additional features, but sticking to JPA ensures vendor neutrality and portability.
- Choose between inheritance and composition based on your domain model:
  - Use **inheritance** for shared behavior and hierarchical relationships.
  - Use **composition** for reusable and flexible field definitions.
