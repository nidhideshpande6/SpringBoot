```java
@Entity
@Data
@AllArgsConstructor
@NoArgsConstructor
@Builder
@Table(
        name = "tbl_student",
        uniqueConstraints = @UniqueConstraint(
                name = "emailid_unique",
                columnNames = "email_address"
        )
)
public class Student {

    @Id
    @SequenceGenerator(
            name = "student_sequence",
            sequenceName = "student_sequence",
            allocationSize = 1
    )
    @GeneratedValue(
            strategy = GenerationType.SEQUENCE,
            generator = "student_sequence"
    )
    private Long studentId;
    private String firstName;
    private String lastName;

    @Column(
            name = "email_address",
            nullable = false
    )
    private String emailId;

    @Embedded
    private Guardian guardian;
}
```
## Explanation of the Student Entity Class
This Java class, Student, represents a database entity for storing student-related information in a relational database using the Java Persistence API (JPA). 
The class uses several annotations from JPA to map the entity to a database table and configure its behavior.

**Below is a detailed breakdown of each part of the class and its annotations:**

## 1. Annotations for Class-level Configuration
`@Entity:`

- This annotation is used to mark the class as a JPA entity, which means it will be mapped to a table in the database.

- The class is a representation of a table in the database, and each instance of the class will correspond to a row in the table.

`@Data:`

- This annotation is from Lombok and automatically generates getters, setters, toString(), equals(), and hashCode() methods for the class at compile-time.

- It's a convenient shortcut to avoid boilerplate code.

`@AllArgsConstructor:`

- Another Lombok annotation that generates a constructor with one parameter for each field in the class.
  
- This constructor can be used to create a Student object with all properties initialized.
  
`@NoArgsConstructor:`

- This Lombok annotation generates a no-argument constructor. It is often used by JPA for entity instantiation during object-relational mapping (ORM).

`@Builder:`

- This Lombok annotation provides the builder pattern, which allows for creating instances of the Student class using a more readable and flexible approach.

#### Example usage: Student.builder().firstName("John").lastName("Doe").build();

`@Table(name = "tbl_student", uniqueConstraints = @UniqueConstraint(name = "emailid_unique", columnNames = "email_address")):`

- The @Table annotation is used to specify the name of the table in the database, in this case, tbl_student.
  
- uniqueConstraints specifies that the email_address column should have a unique constraint, ensuring that each email address in the table is unique.
  
- The unique constraint is named emailid_unique and is applied to the email_address column.
  
## 2. Primary Key and Sequence Generator
`@Id:`

- Marks the studentId field as the primary key of the entity. The primary key uniquely identifies each record in the database table.

`@SequenceGenerator:`

- Configures a sequence generator for generating unique values for the primary key (studentId).
  
   `name = "student_sequence": The name of the sequence generator.`
  
   `sequenceName = "student_sequence": The name of the sequence in the database that generates the ID.`
  
   `allocationSize = 1: Specifies that the sequence will increment by 1 for each new student record.`

`@GeneratedValue:`

- Specifies the strategy for generating the primary key value. In this case, the GenerationType.SEQUENCE strategy is used.

- strategy = GenerationType.SEQUENCE: Specifies that the value of the primary key will be generated using a database sequence.

- generator = "student_sequence": Specifies that the student_sequence sequence generator should be used.

## 3. Attributes of the Student Entity

`private Long studentId;:`

The unique identifier for each student. This field is mapped to the primary key column in the database.

`private String firstName;:`

A field to store the first name of the student. This field will be mapped to a column in the table.

`private String lastName;:`

A field to store the last name of the student. This field will also be mapped to a column in the table.

`@Column(name = "email_address", nullable = false):`

The emailId field is mapped to the email_address column in the database table.

`name = "email_address": Specifies the name of the column.`

nullable = false: This means that the email address cannot be null, and it is a required field.

`private String emailId;:`

The email address of the student. It is a mandatory field due to the nullable = false constraint.

`@Embedded:`

- The guardian field is an embedded object. This means that the attributes of the Guardian class will be included as columns in the Student table.
- The Guardian class is not shown in this code snippet, but it is expected to be a separate class annotated with @Embeddable. This means that its fields (e.g., name, phone number, etc.) will be mapped as part of the Student entity in the database.

## 4. Summary of Key Concepts

- JPA Annotations: The class uses JPA annotations (@Entity, @Table, @Id, @Column, @GeneratedValue, @SequenceGenerator, @Embedded) to define how it should be mapped to the database.

- Lombok: Lombok annotations (@Data, @AllArgsConstructor, @NoArgsConstructor, @Builder) are used to reduce boilerplate code and make the class more readable and maintainable.

- Primary Key Generation: The studentId field uses a sequence generator for unique ID generation, ensuring each student has a unique identifier.

- Unique Constraint: The email_address field is constrained to be unique across all rows in the table, ensuring no two students have the same email address.

- Embedded Objects: The guardian is an embedded object, meaning its fields will be included in the Student table, but it is a separate entity that can be reused in other classes.
