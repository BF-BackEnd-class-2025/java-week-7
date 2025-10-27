# 🧾 OpenCSV 

> **OpenCSV** is a fast, reliable, and easy-to-use library for
> reading, writing, and mapping **CSV files** to Java objects and back.

---

## 📦 1. What Is OpenCSV?

OpenCSV simplifies working with **Comma-Separated Values (CSV)** files in Java.

It supports:

* Reading and writing CSVs
* Handling quoted values & delimiters
* Mapping CSV rows directly to Java classes (Beans)
* Annotations for cleaner mapping
* Converting between objects and CSV (serialization)

---

## ⚙️ 2. Add the Dependency

In your `pom.xml`:

```xml
<dependency>
  <groupId>com.opencsv</groupId>
  <artifactId>opencsv</artifactId>
  <version>5.9</version>
</dependency>
```

If using Gradle:

```gradle
implementation 'com.opencsv:opencsv:5.9'
```

---

## 📂 3. Basic Example — Read and Write CSV Files

### ➤ Example CSV file: `movies.csv`

```csv
title,year,genre,rating
Inception,2010,Sci-Fi,8.8
The Godfather,1972,Crime,9.2
```

---

### ➤ Reading CSV line by line

```java
import com.opencsv.CSVReader;
import java.io.FileReader;
import java.util.List;

public class ReadCsvExample 
{
    public static void main(String[] args) 
    {
        String path = "src/main/resources/movies.csv";

        try (CSVReader reader = new CSVReader(new FileReader(path))) 
        {
            List<String[]> rows = reader.readAll();
            for (String[] row : rows) 
            {
                System.out.println(String.join(" | ", row));
            }
        } 
        catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

**Output:**

```
title | year | genre | rating
Inception | 2010 | Sci-Fi | 8.8
The Godfather | 1972 | Crime | 9.2
```

---

### ➤ Writing CSV

```java
import com.opencsv.CSVWriter;
import java.io.FileWriter;

public class WriteCsvExample 
{
    public static void main(String[] args) 
    {
        String path = "src/main/resources/output.csv";

        try (CSVWriter writer = new CSVWriter(new FileWriter(path))) 
        {
            String[] header = {"title", "year", "genre", "rating"};
            String[] movie1 = {"Inception", "2010", "Sci-Fi", "8.8"};
            String[] movie2 = {"The Godfather", "1972", "Crime", "9.2"};

            writer.writeNext(header);
            writer.writeNext(movie1);
            writer.writeNext(movie2);
            System.out.println("✅ CSV written successfully!");
        } 
        catch (Exception e) 
        {
            e.printStackTrace();
        }
    }
}
```

---

## 🧩 4. Reading CSV into Java Objects (Beans)

This is one of OpenCSV’s best features — mapping CSV directly to your model classes.

### ➤ Define your model class

```java
import com.opencsv.bean.CsvBindByName;

public class Movie {
    @CsvBindByName
    private String title;

    @CsvBindByName
    private int year;

    @CsvBindByName
    private String genre;

    @CsvBindByName
    private double rating;

    // Getters and setters
    public String getTitle() { return title; }
    public int getYear() { return year; }
    public String getGenre() { return genre; }
    public double getRating() { return rating; }

    @Override
    public String toString() {
        return String.format("🎬 %s (%d) - %s - ⭐ %.1f", title, year, genre, rating);
    }
}
```

---

### ➤ Reading CSV to beans

```java
import com.opencsv.bean.CsvToBeanBuilder;
import java.io.FileReader;
import java.util.List;

public class ReadBeanExample 
{
    public static void main(String[] args) 
    {
        String path = "src/main/resources/movies.csv";

        try 
        {
            List<Movie> movies = new CsvToBeanBuilder<Movie>(new FileReader(path))
                    .withType(Movie.class)
                    .withIgnoreLeadingWhiteSpace(true)
                    .build()
                    .parse();

            movies.forEach(System.out::println);
        } 
        catch (Exception e) 
        {
            e.printStackTrace();
        }
    }
}
```

**Output:**

```
🎬 Inception (2010) - Sci-Fi - ⭐ 8.8
🎬 The Godfather (1972) - Crime - ⭐ 9.2
```

---

## ✍️ 5. Writing Java Objects to CSV

```java
import com.opencsv.bean.StatefulBeanToCsv;
import com.opencsv.bean.StatefulBeanToCsvBuilder;
import java.io.FileWriter;
import java.util.List;

public class WriteBeanExample 
{
    public static void main(String[] args) 
    {
        String path = "src/main/resources/movies-out.csv";

        List<Movie> movies = List.of(
            new Movie("Inception", 2010, "Sci-Fi", 8.8),
            new Movie("The Godfather", 1972, "Crime", 9.2)
        );

        try (FileWriter writer = new FileWriter(path)) 
        {
            StatefulBeanToCsv<Movie> beanToCsv = new StatefulBeanToCsvBuilder<Movie>(writer).build();
            beanToCsv.write(movies);
            System.out.println("✅ CSV file written successfully!");
        } 
        catch (Exception e) 
        {
            e.printStackTrace();
        }
    }
}
```

---

## ⚙️ 6. Common Annotations

| Annotation               | Description                                   |
|--------------------------|-----------------------------------------------|
| `@CsvBindByName`         | Maps CSV column by header name                |
| `@CsvBindByPosition`     | Maps CSV column by index position             |
| `@CsvIgnore`             | Skips the field                               |
| `@CsvDate("yyyy-MM-dd")` | Converts date strings to `Date` objects       |
| `@CsvBindAndSplitByName` | Handles lists (splits comma-separated values) |

---

## 🧱 7. Handling Different Delimiters

By default, OpenCSV assumes commas (`,`).
You can change it to another delimiter (e.g., semicolon `;` or tab `\t`):

```java
import com.opencsv.CSVReaderBuilder;
import com.opencsv.CSVParserBuilder;

try (CSVReader reader = new CSVReaderBuilder(new FileReader("movies.csv"))
        .withCSVParser(new CSVParserBuilder().withSeparator(';').build())
        .build()) {
    reader.forEach(row -> System.out.println(String.join(" | ", row)));
}
```

---

## 🧩 8. Handling Headers and Skipping Lines

Skip the header row easily:

```java
try (CSVReader reader = new CSVReaderBuilder(new FileReader("movies.csv"))
        .withSkipLines(1)
        .build()) {
    reader.forEach(row -> System.out.println(String.join(", ", row)));
}
```

---

## 🧠 9. Combining OpenCSV with Gson or SLF4J

### 🔹 Convert CSV → JSON

You can use OpenCSV + Gson to convert a CSV file to JSON:

```java
import com.google.gson.Gson;
import com.opencsv.bean.CsvToBeanBuilder;
import java.io.FileReader;
import java.util.List;

public class CsvToJson
{
    public static void main(String[] args) 
    {
        try 
        {
            List<Movie> movies = new CsvToBeanBuilder<Movie>(new FileReader("movies.csv"))
                    .withType(Movie.class)
                    .build()
                    .parse();

            String json = new Gson().toJson(movies);
            System.out.println(json);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

**Output:**

```json
[
  {"title":"Inception","year":2010,"genre":"Sci-Fi","rating":8.8},
  {"title":"The Godfather","year":1972,"genre":"Crime","rating":9.2}
]
```

---

### 🔹 Add logging with SLF4J

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class CsvLogger
{
    private static final Logger logger = LoggerFactory.getLogger(CsvLogger.class);

    public void readCsv(String path) 
    {
        logger.info("📂 Reading CSV from {}", path);
        // read logic here...
    }
}
```

---

## 🧾 10. Common Exceptions & Tips

| Problem                         | Cause                                | Fix                                               |
|---------------------------------|--------------------------------------|---------------------------------------------------|
| `java.io.FileNotFoundException` | Wrong file path                      | Use absolute path or `resources/` folder          |
| `Header mapping mismatch`       | Header name doesn’t match field name | Use `@CsvBindByPosition` or check header spelling |
| Empty fields                    | CSV has blanks                       | Set `.withIgnoreLeadingWhiteSpace(true)`          |
| Wrong delimiter                 | CSV uses `;` not `,`                 | Use `.withSeparator(';')`                         |

---

## 🧱 11. Example Project Structure

```
movie-csv-demo/
│
├── pom.xml
└── src/
    └── main/
        ├── java/
        │   ├── model/Movie.java
        │   ├── app/ReadCsvExample.java
        │   ├── app/WriteCsvExample.java
        │   └── app/CsvToJson.java
        └── resources/
            ├── movies.csv
            └── movies-out.csv
```

---

## ✅ 12. Summary

| Feature             | Description                                                       |
|---------------------|-------------------------------------------------------------------|
| **Library**         | `com.opencsv:opencsv`                                             |
| **Purpose**         | Read/write CSV files easily                                       |
| **Supports**        | Beans, annotations, custom delimiters                             |
| **Main Classes**    | `CSVReader`, `CSVWriter`, `CsvToBeanBuilder`, `StatefulBeanToCsv` |
| **Best For**        | CSV-based data import/export                                      |
| **Version**         | 5.9                                                               |
| **Integrates With** | Gson (JSON), SLF4J (logging), databases, etc.                     |

---

## 💡 13. Pro Tips

* Use `CsvToBeanBuilder` for real-world CSV parsing (instead of `CSVReader` directly).
* Annotate model fields with `@CsvBindByName` for safer mapping.
* Always close readers/writers with try-with-resources.
* Use `StatefulBeanToCsvBuilder` to export lists cleanly.
* Works perfectly alongside **Gson** (for JSON export) and **DBCP2** (for DB import/export).

---

## 🎬 14. Example Combined Output

```
🎬 Inception (2010) - Sci-Fi - ⭐ 8.8
🎬 The Godfather (1972) - Crime - ⭐ 9.2
✅ CSV file written successfully!
📂 Converted CSV to JSON successfully!
```

---

