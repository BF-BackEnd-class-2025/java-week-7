# 🧱 **Projects**

**Focus:** Modular Design, Dependency Management, Real-World Code Organization

---

## 1. 🧾 Expense Tracker (Maven Edition)

**Goal:** Build a CLI app to record daily expenses and store them in a PostgreSQL database.

**Concepts:** JDBC + Connection Pooling + Logging + `config.properties`

**Dependencies:**

```xml
<dependency>
  <groupId>org.postgresql</groupId>
  <artifactId>postgresql</artifactId>
  <version>42.7.3</version>
</dependency>
<dependency>
  <groupId>org.apache.commons</groupId>
  <artifactId>commons-dbcp2</artifactId>
  <version>2.11.0</version>
</dependency>
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-simple</artifactId>
  <version>2.0.12</version>
</dependency>
```

**Instructions:**

1. Create a table `expenses(id SERIAL, category VARCHAR, amount DECIMAL, date DATE)`.
2. Load DB credentials from `src/main/resources/config.properties`.
3. Use `DBCP2` for connection pooling (no need for manual `DriverManager`).
4. Implement CRUD operations:

    * Add new expense
    * Update expense amount
    * Delete an expense
    * View all expenses
5. Log every database action using `slf4j`.

📦 **Packages:**

* `model` → `Expense.java`
* `dao` → `ExpenseDAO.java`
* `service` → `ExpenseService.java`
* `util` → `DBUtil.java`, `ConfigReader.java`
* `app` → `App.java`

---

## 2. 🎬 Movie Catalog App with JSON

**Goal:** Manage a movie list using JSON files instead of a database.

**Concepts:** File I/O + Gson + Object Mapping

**Dependencies:**

```xml
<dependency>
  <groupId>com.google.code.gson</groupId>
  <artifactId>gson</artifactId>
  <version>2.11.0</version>
</dependency>
```

**Instructions:**

1. Define a `Movie` class with title, year, genre, and rating.
2. Create a menu:

    * Add new movie
    * Delete movie
    * List all movies
    * Search by genre
3. Store and load all data from `movies.json` using Gson.

📦 **Packages:**

* `model` → `Movie.java`
* `service` → `MovieService.java`
* `util` → `JsonUtil.java`
* `app` → `App.java`

---

## 3. 📈 Employee Payroll + Database

**Goal:** Manage employees and salaries in a database using the DAO pattern.

**Concepts:** JDBC + DAO + Resource Management

**Dependencies:**

```xml
<dependency>
  <groupId>org.postgresql</groupId>
  <artifactId>postgresql</artifactId>
  <version>42.7.3</version>
</dependency>
<dependency>
  <groupId>org.apache.commons</groupId>
  <artifactId>commons-dbcp2</artifactId>
  <version>2.11.0</version>
</dependency>
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-simple</artifactId>
  <version>2.0.12</version>
</dependency>
```

**Instructions:**

1. Create table `employees(id SERIAL, name VARCHAR, position VARCHAR, salary DECIMAL)`.
2. Implement DAO methods:

    * `addEmployee()`
    * `updateSalary()`
    * `deleteEmployee()`
    * `getAllEmployees()`
3. Use `DBUtils` for cleaner resource handling.
4. Use transactions for salary updates.

📦 **Packages:**

* `model`, `dao`, `service`, `util`, `app`

---

## 4. 🛍 Shopping Cart (CLI App)

**Goal:** Simulate a shopping cart where items are saved to JSON.

**Concepts:** Gson + Collections + File Persistence

**Dependencies:**

```xml
<dependency>
  <groupId>com.google.code.gson</groupId>
  <artifactId>gson</artifactId>
  <version>2.11.0</version>
</dependency>
```

**Instructions:**

1. Define `Product(id, name, price, quantity)`.
2. Implement:

    * Add product to cart
    * Remove product
    * Show cart total
3. Save all cart data to `cart.json` using Gson.
4. Load previous cart on startup.

📦 **Packages:**
`model`, `service`, `util`, `app`

---

## 5. 🧮 Student Report Generator

**Goal:** Generate student grade reports as CSV files.

**Concepts:** JDBC + OpenCSV + File Writing

**Dependencies:**

```xml
<dependency>
  <groupId>org.postgresql</groupId>
  <artifactId>postgresql</artifactId>
  <version>42.7.3</version>
</dependency>
<dependency>
  <groupId>com.opencsv</groupId>
  <artifactId>opencsv</artifactId>
  <version>5.9</version>
</dependency>
```

**Instructions:**

1. Create table `students(id, name, subject, grade)`.
2. Fetch all records with JDBC.
3. Export to `report.csv` using OpenCSV.
4. Display total average per student.

📦 **Packages:**
`model`, `dao`, `service`, `util`, `app`

---

## 6. 🧠 Quiz System (Maven Project)

**Goal:** Build a quiz app that loads questions from JSON.

**Concepts:** Gson + OOP + JUnit testing

**Dependencies:**

```xml
<dependency>
  <groupId>com.google.code.gson</groupId>
  <artifactId>gson</artifactId>
  <version>2.11.0</version>
</dependency>
<dependency>
  <groupId>org.junit.jupiter</groupId>
  <artifactId>junit-jupiter-api</artifactId>
  <version>5.11.2</version>
  <scope>test</scope>
</dependency>
```

**Instructions:**

1. Create `Question(questionText, options[], correctAnswer)` class.
2. Load questions from `questions.json`.
3. Present quiz randomly to the user.
4. Count score and show results.
5. Add 2–3 JUnit tests for quiz logic.

📦 **Packages:**
`model`, `service`, `util`, `app`, `test`

---

## 7. 🏦 Bank Account Simulator (Maven Version)

**Goal:** Simulate a small banking system using JDBC transactions.

**Concepts:** Transactions + Logging + Error Handling

**Dependencies:**

```xml
<dependency>
  <groupId>org.postgresql</groupId>
  <artifactId>postgresql</artifactId>
  <version>42.7.3</version>
</dependency>
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-simple</artifactId>
  <version>2.0.12</version>
</dependency>
```

**Instructions:**

1. Create table `accounts(id SERIAL, name VARCHAR, balance DECIMAL)`.
2. Implement operations:

    * Deposit
    * Withdraw
    * Transfer between accounts
3. Use transactions with rollback on insufficient funds.
4. Log every transaction.

📦 **Packages:**
`model`, `dao`, `service`, `util`, `app`

---

## 8. ⚙️ Configuration Manager (Serializable Version)

**Goal:** Manage app settings and save them automatically.

**Concepts:** Gson + Serialization + File I/O

**Dependencies:**

```xml
<dependency>
  <groupId>com.google.code.gson</groupId>
  <artifactId>gson</artifactId>
  <version>2.11.0</version>
</dependency>
```

**Instructions:**

1. Create a `Settings` class (theme, language, volume).
2. On startup → load from `settings.json`.
3. When user changes a setting → auto-save with Gson.
4. Test serialization manually by inspecting file.

📦 **Packages:**
`model`, `service`, `util`, `app`

---

## 9. 🧰 Task Manager App

**Goal:** Manage personal tasks stored in JSON files.

**Concepts:** Gson + CRUD + OOP + Logging

**Dependencies:**

```xml
<dependency>
  <groupId>com.google.code.gson</groupId>
  <artifactId>gson</artifactId>
  <version>2.11.0</version>
</dependency>
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-simple</artifactId>
  <version>2.0.12</version>
</dependency>
```

**Instructions:**

1. Define `Task(id, title, dueDate, completed)`.
2. Implement:

    * Add task
    * Mark as done
    * Delete task
    * List all tasks
3. Store in `tasks.json`.
4. Log actions (added, removed, completed).

📦 **Packages:**
`model`, `service`, `util`, `app`

---

## 10. 🗃 CRUD Console App (Database + Maven)

**Goal:** A professional-style CRUD project integrating everything.

**Concepts:** JDBC + Config + DAO + Logging + Layers

**Dependencies:**

```xml
<dependency>
  <groupId>org.postgresql</groupId>
  <artifactId>postgresql</artifactId>
  <version>42.7.3</version>
</dependency>
<dependency>
  <groupId>org.apache.commons</groupId>
  <artifactId>commons-dbcp2</artifactId>
  <version>2.11.0</version>
</dependency>
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-simple</artifactId>
  <version>2.0.12</version>
</dependency>
```

**Instructions:**

1. Create table `users(id SERIAL, name VARCHAR, email VARCHAR, age INT)`.
2. Implement full CRUD:

    * Create user
    * Read all users
    * Update user info
    * Delete user
3. Use DBCP2 connection pool.
4. Use `config.properties` for DB connection.
5. Apply clean packaging and `App.java` as entry point.

📦 **Packages:**

```
src/main/java/com/app/
 ├── model/
 ├── dao/
 ├── service/
 ├── util/
 ├── App.java
```

---

## 💡 Final Tip for Students

**End of Week 7 goals:**

* Understand Maven project structure
* Add & manage dependencies confidently
* Organize Java code into layers (`model`, `dao`, `service`, `app`)
* Work with JSON, CSV, and Databases
* Use logging and configuration files like real developers

---

