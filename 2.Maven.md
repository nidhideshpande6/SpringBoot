## Steps to Create a Maven Project in IntelliJ IDEA

### 1. Open IntelliJ IDEA
- Launch the IntelliJ IDEA IDE.

### 2. Start a New Project
- Navigate to the top menu bar and click on **File -> New Project**.

### 3. Select Maven as the Project Type
- In the **New Project** wizard:
  - Choose **Maven** from the list of project types.
  - Ensure the correct Java SDK is selected.

### 4. Configure Maven Project Details
- Enter the required fields:
  - **GroupId**: Identifies your project's group (e.g., `com.example`).
  - **ArtifactId**: Name of your project (e.g., `my-app`).
  - **Version**: Default version of the project (e.g., `1.0-SNAPSHOT`).

### 5. Finalize Project Creation
- Click **Finish** to generate the project structure.
- IntelliJ IDEA will automatically download dependencies and set up the workspace for you.

---



# Maven - Build Automation and Project Management Tool

## What is Maven?

Maven is a **build automation and project management tool** widely used in Java-based projects. It simplifies the process of building, testing, and deploying applications by managing dependencies, project configurations, and lifecycle phases.

---

## Key Concepts

### 1. Project Object Model (POM)
- The **`pom.xml`** file is the core of Maven. It contains:
  - Project metadata (GroupId, ArtifactId, Version)
  - Dependencies
  - Plugins
  - Build and deployment configurations

### 2. Dependency Management
- Maven automatically downloads and manages libraries (JAR files) required for your project from a central repository or custom repositories.
- Transitive dependencies are also resolved automatically.

### 3. Standard Project Structure
Maven enforces a standard directory layout, making it easy to understand project structures:
src/ main/ java/ # Source code resources/ # Configuration files test/ java/ # Test source code resources/ # Test configuration pom.xml # Project Object Model file

markdown
Copy code

### 4. Build Lifecycle
Maven automates various phases of the development lifecycle:
- **Compile:** `mvn compile` – Compiles the source code.
- **Test:** `mvn test` – Runs unit tests.
- **Package:** `mvn package` – Packages the compiled code into JAR/WAR files.
- **Install:** `mvn install` – Installs the package into the local repository.
- **Deploy:** `mvn deploy` – Deploys the package to a remote repository.

### 5. Plugins and Goals
- Maven uses plugins to perform specific tasks (e.g., compiling, packaging, testing).
- **Goals** are tasks executed by plugins (e.g., `clean`, `install`).

---

## Why Use Maven?

### Benefits:
1. **Simplified Dependency Management**  
   Maven fetches libraries automatically and handles version conflicts.
2. **Standardized Project Structure**  
   Enforces best practices and improves maintainability.
3. **Portability**  
   Works seamlessly across different environments.
4. **Extensibility**  
   Supports plugins for additional functionality (e.g., reporting, code analysis).
5. **Integration**  
   Integrates easily with IDEs (e.g., IntelliJ IDEA, Eclipse) and CI tools (e.g., Jenkins).

### Drawbacks:
- Initial learning curve.
- Overhead for small-scale projects.

---

## How Maven Works

1. **Dependencies and Plugins**  
   Maven retrieves dependencies and plugins from repositories (e.g., Maven Central).
2. **POM Configuration**  
   Dependencies and project settings are declared in `pom.xml`.
3. **Command-Line Execution**  
   Common Maven commands:
   - `mvn clean` – Cleans the target directory.
   - `mvn compile` – Compiles the source code.
   - `mvn test` – Executes unit tests.
   - `mvn package` – Packages the application (JAR/WAR).
   - `mvn install` – Installs the package into the local repository.
4. **Build Output**  
   Maven builds the application and creates artifacts like JAR/WAR files for deployment.

---

## Getting Started with Maven

### Prerequisites:
- Install Java JDK.
- Install Maven ([Download Maven](https://maven.apache.org/download.cgi)).

### Basic Maven Commands:
```bash
# Clean the project
mvn clean

# Compile the project
mvn compile

# Run unit tests
mvn test

# Package the project
mvn package

# Install the package locally
mvn install
Example pom.xml:
xml
Copy code
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>my-app</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
            <version>3.1.2</version>
        </dependency>
    </dependencies>
</project>
