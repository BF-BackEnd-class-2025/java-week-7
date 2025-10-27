# 🧪 JUnit Jupiter 

> **JUnit 5** (code-named *Jupiter*) is the most popular **unit testing framework for Java**.
> It helps developers write reliable, automated tests for classes, methods, and logic.

---

## ⚙️ 1. Add the Dependency

Add this to your **`pom.xml`**:

```xml
<dependency>
  <groupId>org.junit.jupiter</groupId>
  <artifactId>junit-jupiter-api</artifactId>
  <version>5.11.2</version>
  <scope>test</scope>
</dependency>

<dependency>
  <groupId>org.junit.jupiter</groupId>
  <artifactId>junit-jupiter-engine</artifactId>
  <version>5.11.2</version>
  <scope>test</scope>
</dependency>
```

💡 `junit-jupiter-api` gives you the annotations and methods.
`junit-jupiter-engine` is the runtime that actually executes the tests.

If using Gradle:

```gradle
testImplementation 'org.junit.jupiter:junit-jupiter-api:5.11.2'
testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine:5.11.2'
```

---

## 🧠 2. What Is Unit Testing?

Unit testing means testing **small pieces of code in isolation** — typically one method or one class at a time.

✅ **Goals:**

* Verify correctness of logic
* Prevent regressions
* Speed up development
* Improve code confidence

---

## 🧩 3. Basic Test Example

Suppose you have a class:

```java
package service;

public class MathService {
    public int add(int a, int b) {
        return a + b;
    }

    public boolean isEven(int num) {
        return num % 2 == 0;
    }
}
```

### ➤ Create a test class

```java
package service;

import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class MathServiceTest {

    private final MathService math = new MathService();

    @Test
    void testAdd() {
        assertEquals(5, math.add(2, 3), "2 + 3 should equal 5");
    }

    @Test
    void testIsEven() {
        assertTrue(math.isEven(4));
        assertFalse(math.isEven(5));
    }
}
```

Run it via:

```bash
mvn test
```

✅ **Output:**

```
[INFO] Tests run: 2, Failures: 0, Errors: 0, Skipped: 0
```

---

## 🧱 4. Common Annotations

| Annotation     | Description                         | Example                                  |
|----------------|-------------------------------------|------------------------------------------|
| `@Test`        | Marks a test method                 | `@Test void myTest() {}`                 |
| `@BeforeEach`  | Runs before each test               | Setup logic                              |
| `@AfterEach`   | Runs after each test                | Cleanup logic                            |
| `@BeforeAll`   | Runs once before all tests (static) | DB setup                                 |
| `@AfterAll`    | Runs once after all tests (static)  | DB teardown                              |
| `@DisplayName` | Custom test name                    | `@DisplayName("Should add two numbers")` |
| `@Disabled`    | Temporarily skip a test             | `@Disabled("TODO: Fix later")`           |

---

### Example with lifecycle annotations

```java
import org.junit.jupiter.api.*;

@TestInstance(TestInstance.Lifecycle.PER_CLASS)
class ExampleLifecycleTest {

    @BeforeAll
    void beforeAll() {
        System.out.println("🚀 Runs once before all tests");
    }

    @BeforeEach
    void beforeEach() {
        System.out.println("🔹 Runs before each test");
    }

    @Test
    void testA() {
        System.out.println("✅ Running Test A");
    }

    @Test
    void testB() {
        System.out.println("✅ Running Test B");
    }

    @AfterEach
    void afterEach() {
        System.out.println("🔹 Runs after each test");
    }

    @AfterAll
    void afterAll() {
        System.out.println("🧹 Runs once after all tests");
    }
}
```

---

## 📜 5. Assertions — Validating Test Results

| Assertion                        | Purpose                   | Example                                                         |
|----------------------------------|---------------------------|-----------------------------------------------------------------|
| `assertEquals(expected, actual)` | Compare values            | `assertEquals(10, calc.add(5,5))`                               |
| `assertTrue(condition)`          | Assert boolean true       | `assertTrue(user.isActive())`                                   |
| `assertFalse(condition)`         | Assert boolean false      | `assertFalse(list.isEmpty())`                                   |
| `assertNull(obj)`                | Check for `null`          | `assertNull(result)`                                            |
| `assertNotNull(obj)`             | Check not `null`          | `assertNotNull(movie)`                                          |
| `assertThrows()`                 | Expect an exception       | `assertThrows(IOException.class, () -> readFile())`             |
| `assertAll()`                    | Group multiple assertions | `assertAll(() -> assertTrue(x>0), () -> assertEquals("Hi", s))` |

---

## 🎯 6. Parameterized Tests

You can run the same test with multiple inputs using `@ParameterizedTest`.

### ➤ Add parameterized dependency

```xml
<dependency>
  <groupId>org.junit.jupiter</groupId>
  <artifactId>junit-jupiter-params</artifactId>
  <version>5.11.2</version>
  <scope>test</scope>
</dependency>
```

### ➤ Example

```java
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.ValueSource;

import static org.junit.jupiter.api.Assertions.assertTrue;

class ParameterizedExampleTest {

    @ParameterizedTest
    @ValueSource(ints = {2, 4, 6, 8})
    void testEvenNumbers(int number) {
        assertTrue(number % 2 == 0, number + " should be even");
    }
}
```

---

## 🧩 7. Testing Exceptions

```java
@Test
void testDivideByZero() {
    ArithmeticException ex = assertThrows(ArithmeticException.class, () -> {
        int result = 10 / 0;
    });
    assertEquals("/ by zero", ex.getMessage());
}
```

---

## 🧠 8. Example: Testing Gson Code

You can test if your Gson serialization works as expected:

```java
import com.google.gson.Gson;
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class GsonTest {
    static class Movie {
        String title;
        int year;
    }

    @Test
    void testToJson() {
        Movie m = new Movie();
        m.title = "Inception";
        m.year = 2010;

        String json = new Gson().toJson(m);
        assertTrue(json.contains("Inception"));
    }

    @Test
    void testFromJson() {
        String json = "{\"title\":\"Inception\",\"year\":2010}";
        Movie m = new Gson().fromJson(json, Movie.class);
        assertEquals("Inception", m.title);
    }
}
```

---

## 🧰 9. Example: Testing Database Connection (DBCP2)

```java
import org.apache.commons.dbcp2.BasicDataSource;
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class DbConnectionTest {
    @Test
    void testConnectionSetup() throws Exception {
        BasicDataSource ds = new BasicDataSource();
        ds.setUrl("jdbc:h2:mem:testdb");
        ds.setUsername("sa");
        ds.setPassword("");
        ds.setDriverClassName("org.h2.Driver");

        assertNotNull(ds.getConnection());
        ds.close();
    }
}
```

✅ Uses an in-memory H2 database for fast, isolated testing.

---

## 🧪 10. Running Tests

### With Maven:

```bash
mvn test
```

### With Gradle:

```bash
gradle test
```

### In IntelliJ / VS Code:

* Right-click the test class or method → **Run Test**

---

## 📂 11. Recommended Project Structure

```
movie-catalog/
│
├── pom.xml
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   ├── app/App.java
│   │   │   └── service/MovieService.java
│   │   └── resources/movies.json
│   └── test/
│       └── java/
│           └── service/
│               └── MovieServiceTest.java
```

JUnit automatically detects test files inside `src/test/java`.

---

## 🧾 12. Integration with Maven Surefire

Maven uses **Surefire Plugin** to run tests automatically.
If not included, add it to your `pom.xml`:

```xml
<build>
  <plugins>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-surefire-plugin</artifactId>
      <version>3.2.5</version>
    </plugin>
  </plugins>
</build>
```

---

## 📊 13. Test Reporting

After running `mvn test`, reports are generated in:

```
target/surefire-reports/
```

You’ll find files like:

```
TEST-service.MathServiceTest.xml
```

You can use these reports for CI/CD pipelines or HTML dashboards.

---

## ✅ 14. Summary

| Feature              | Description                                               |
|----------------------|-----------------------------------------------------------|
| **Library**          | `org.junit.jupiter:junit-jupiter-api`                     |
| **Version**          | 5.11.2                                                    |
| **Purpose**          | Write and run unit tests                                  |
| **Main Annotations** | `@Test`, `@BeforeEach`, `@AfterAll`, `@ParameterizedTest` |
| **Assertions**       | `assertEquals`, `assertTrue`, `assertThrows`, etc.        |
| **Compatible With**  | Maven, Gradle, IntelliJ, Eclipse, VS Code                 |
| **Integration**      | Works perfectly with Gson, DBCP2, SLF4J, and OpenCSV      |
| **Test Scope**       | `<scope>test</scope>` ensures it’s used only for tests    |

---

## 💡 15. Pro Tips

* Keep your **tests small and isolated**.
* Use **mock data** (not production files).
* Run tests automatically on every build (`mvn test`).
* Use `@DisplayName` for readable test names.
* Combine with **Mockito** for mocking dependencies (optional).
* Keep your `src/test/java` structure identical to `src/main/java`.

---

## 🧩 Example Output (All Tests Passing)

```
[INFO] Running service.MathServiceTest
[INFO] Tests run: 2, Failures: 0, Errors: 0, Skipped: 0
[INFO] 
[INFO] BUILD SUCCESS
```

---
