# Spring and Java Annotations: Detailed Q&A

### 1. **Which of the following acts as an IOC container?**
**Options:**
1. ApplicationContext
2. DispatcherServlet
3. SpringRunner
4. BeanFactory
5. None

**Answer:** 1 and 4 (ApplicationContext and BeanFactory)

**Explanation:**
- **IOC (Inversion of Control)** container is responsible for managing the lifecycle and dependencies of Spring beans.
- **ApplicationContext**: A more feature-rich IOC container compared to BeanFactory, offering advanced features like event propagation and declarative mechanisms.
- **BeanFactory**: A simpler IOC container that provides the basic functionality of dependency injection.
- Other options:
  - **DispatcherServlet**: Handles HTTP requests in Spring MVC and is not an IOC container.
  - **SpringRunner**: A JUnit runner for Spring tests and not an IOC container.
  - **None**: Incorrect, as valid IOC containers are present in the list.

---

### 2. **Thread 1 acquires payment lock and waits for order lock, while Thread 2 acquires order lock and waits for payment lock. What happens?**
**Options:**
1. Both lock without issue
2. Deadlock
3. It prints whatever there
4. Compile error

**Answer:** 2. Deadlock

**Explanation:**
- **Deadlock** occurs when two threads are waiting for each other to release locks they need to proceed.
- Thread 1 holds the payment lock and waits for the order lock, while Thread 2 holds the order lock and waits for the payment lock, creating a circular dependency.
- No thread can proceed, causing a deadlock.

---

### 3. **Default priority of a thread in Java?**
**Answer:** 5 (NORM_PRIORITY)

**Explanation:**
- Thread priorities in Java range from 1 (MIN_PRIORITY) to 10 (MAX_PRIORITY).
- By default, a thread is assigned a priority of 5, which is considered NORM_PRIORITY.
- Thread priorities can be changed using the `setPriority()` method, but their effect depends on the underlying operating system's thread scheduler.

---

### 4. **Significance of the `volatile` keyword in Java?**
**Options:**
1. It ensures that a variable is visible to all threads.
2. It prevents multiple threads from accessing a variable.
3. It ensures that a variable is only read once.
4. It creates a copy of a variable for each thread.

**Answer:** 1. It ensures that a variable is visible to all threads.

**Explanation:**
- The `volatile` keyword ensures that updates to a variable are immediately visible to all threads.
- Other options:
  - **2**: Incorrect. `volatile` does not prevent access; it ensures visibility.
  - **3**: Incorrect. It doesn’t limit how many times a variable can be read.
  - **4**: Incorrect. All threads share the same volatile variable; no separate copies are created.

---

### 5. **Correct way to run a task asynchronously in Spring?**
**Answer:** Use `@Async` annotation along with `@EnableAsync`.

**Explanation:**
1. **@EnableAsync**: Enables asynchronous processing in Spring.
2. **@Async**: Marks a method to be executed asynchronously.

**Example:**
```java
@Configuration
@EnableAsync
public class AsyncConfig {}

@Service
public class MyService {
    @Async
    public void performTask() {
        System.out.println("Running asynchronously");
    }
}
```
To trigger:
```java
@Autowired
private MyService myService;
myService.performTask(); // Runs asynchronously
```

---

### 6. **Use of `@ControllerAdvice`?**
**Options:**
1. To apply global exception handling
2. To manage async methods
3. To execute scheduled tasks
4. To create RESTful web services

**Answer:** 1. To apply global exception handling

**Explanation:**
- `@ControllerAdvice` is used for centralizing exception handling across multiple controllers.
- Example:
  ```java
  @ControllerAdvice
  public class GlobalExceptionHandler {
      @ExceptionHandler(Exception.class)
      public ResponseEntity<String> handleException(Exception e) {
          return new ResponseEntity<>("Error: " + e.getMessage(), HttpStatus.INTERNAL_SERVER_ERROR);
      }
  }
  ```
- Other options:
  - **2**: Async methods are managed using `@Async`.
  - **3**: Scheduled tasks are managed using `@Scheduled`.
  - **4**: RESTful web services are created using `@RestController`.

---
