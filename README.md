# Week 7: Maven & Project Structure

This week introduces **Apache Maven**, a build automation and dependency management tool that helps
Java developers organize, build, and maintain projects efficiently.  
Students will learn how to use Maven to manage external libraries, set up clean project structures,
and prepare their codebase for frameworks like Spring Boot.

---

## Topics Covered

### 1. What is Maven & Why Use It
- Maven as a **build and dependency management tool**
- Benefits over manual `.jar` handling
- Maven lifecycle: `clean`, `compile`, `package`, `install`

### 2. Setting Up a Maven Project
- Creating a Maven project in IntelliJ IDEA
- Understanding the **standard folder structure**:
```

src/main/java        → source code
src/main/resources   → config files
src/test/java        → tests
pom.xml              → project configuration

```
- Running Maven goals and commands

### 3. Dependency Management
- Adding external libraries through the `pom.xml` file
- Using [Maven Central Repository](https://mvnrepository.com/)
- Common dependencies:  
- PostgreSQL (JDBC)  
- Gson (JSON)  
- OpenCSV (CSV handling)  
- SLF4J (logging)  
- JUnit (testing)

### 4. Organizing Packages & Modules
- Applying a **clean, layered architecture**:
```

model/     → Data classes
dao/       → Database access
service/   → Business logic
util/      → Helpers (DB, JSON, etc.)
App.java   → Main entry point

```
- Using `resources/config.properties` for DB credentials and settings

---

## 🎯 Learning Outcomes

By the end of Week 7, students will be able to:
- Understand the purpose and advantages of Maven  
- Create and configure Maven projects in IntelliJ  
- Add and manage dependencies via `pom.xml`  
- Use Maven commands to compile, package, and run applications  
- Structure Java code into modular packages (`model`, `dao`, `service`, `util`)  
- Load configurations from resource files instead of hardcoding  
- Apply Maven in small real-world projects (JSON, CSV, JDBC)

---

## 🧰 Recommended Tools

| Tool                   | Purpose                                    |
|------------------------|--------------------------------------------|
| **IntelliJ IDEA**      | Create and manage Maven projects           |
| **Maven**              | Build automation and dependency management |
| **PostgreSQL / MySQL** | Database for JDBC projects                 |
| **pgAdmin / DBeaver**  | Database GUI tools                         |
| **Maven Central**      | Find and copy dependency code snippets     |

---