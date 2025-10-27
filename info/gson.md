# 🧠 Gson 

> **Gson** is a Java library developed by **Google** for converting **Java objects ↔ JSON**.
>
> It’s lightweight, fast, and easy to use — perfect for APIs, configuration files, and data persistence.

---

## 📦 1. What Is Gson?

Gson allows you to:

* Serialize Java objects → JSON (save/export)
* Deserialize JSON → Java objects (load/import)
* Work with JSON trees and custom types
* Handle generics and nested structures easily

It’s one of the most popular libraries for JSON handling in Java (used in Android, Spring Boot, etc.).

---

## ⚙️ 2. Add the Dependency

If you’re using **Maven**, add this to your `pom.xml`:

```xml
<dependency>
  <groupId>com.google.code.gson</groupId>
  <artifactId>gson</artifactId>
  <version>2.11.0</version>
</dependency>
```

If you’re using **Gradle**:

```gradle
implementation 'com.google.code.gson:gson:2.11.0'
```

---

## 🧩 3. Basic Usage Example

### ➤ Step 1: Create a Java class

```java
public class Movie 
{
    private String title;
    private int year;
    private String genre;
    private double rating;

    public Movie(String title, int year, String genre, double rating) 
    {
        this.title = title;
        this.year = year;
        this.genre = genre;
        this.rating = rating;
    }

    // Getters & setters (optional)
}
```

### ➤ Step 2: Convert Java object → JSON

```java
import com.google.gson.Gson;

public class GsonExample 
{
    public static void main(String[] args) 
    {
        Movie movie = new Movie("Inception", 2010, "Sci-Fi", 8.8);
        Gson gson = new Gson();

        String json = gson.toJson(movie);
        System.out.println(json);
    }
}
```

**Output:**

```json
{"title":"Inception","year":2010,"genre":"Sci-Fi","rating":8.8}
```

### ➤ Step 3: Convert JSON → Java object

```java
String json = "{\"title\":\"Inception\",\"year\":2010,\"genre\":\"Sci-Fi\",\"rating\":8.8}";
Movie movie = gson.fromJson(json, Movie.class);
System.out.println(movie.getTitle()); // Inception
```

---

## 📜 4. Gson Methods Summary

| Method                       | Description                                          | Example                                          |
|------------------------------|------------------------------------------------------|--------------------------------------------------|
| `toJson(Object)`             | Converts Java object to JSON                         | `gson.toJson(user)`                              |
| `fromJson(String, Class<T>)` | Converts JSON string to object                       | `gson.fromJson(json, User.class)`                |
| `fromJson(Reader, Type)`     | Converts JSON from file or stream                    | `gson.fromJson(reader, listType)`                |
| `new GsonBuilder()`          | For customizing Gson (pretty print, exclusion, etc.) | `new GsonBuilder().setPrettyPrinting().create()` |

---

## 📂 5. Working with Collections (List, Map)

When deserializing a generic type (like a List), you need to use **TypeToken** to preserve type info.

### ➤ Example: List of movies

```java
import com.google.gson.reflect.TypeToken;
import java.lang.reflect.Type;
import java.util.List;

String json = "[{\"title\":\"Inception\",\"year\":2010,\"genre\":\"Sci-Fi\",\"rating\":8.8}]";

Type movieListType = new TypeToken<List<Movie>>() {}.getType();
List<Movie> movies = gson.fromJson(json, movieListType);

for (Movie m : movies) 
{
    System.out.println(m.getTitle());
}
```

---

## 🎨 6. Pretty Printing JSON

Make your JSON human-readable (formatted with spaces and line breaks):

```java
Gson gson = new GsonBuilder().setPrettyPrinting().create();
String json = gson.toJson(movie);
System.out.println(json);
```

**Output:**

```json
{
  "title": "Inception",
  "year": 2010,
  "genre": "Sci-Fi",
  "rating": 8.8
}
```

---

## 🧱 7. Reading & Writing JSON Files

Example of how Gson integrates with **File I/O** (like in your Movie Catalog project):

```java
import com.google.gson.Gson;
import com.google.gson.reflect.TypeToken;
import java.io.*;
import java.lang.reflect.Type;
import java.util.List;

public class JsonUtil 
{
    private static final Gson gson = new Gson();

    public static <T> List<T> loadFromFile(String filename, Type typeOfT) 
    {
        try (Reader reader = new FileReader(filename)) 
        {
            return gson.fromJson(reader, typeOfT);
        } 
        catch (IOException e) 
        {
            e.printStackTrace();
            return List.of();
        }
    }

    public static void saveToFile(List<?> list, String filename) 
    {
        try (Writer writer = new FileWriter(filename)) 
        {
            gson.toJson(list, writer);
        } 
        catch (IOException e) 
        {
            e.printStackTrace();
        }
    }
}
```

---

## ⚡ 8. Handling Nested Objects

You can serialize and deserialize objects that contain other objects:

```java
class Director {
    String name;
    int age;
}

class Movie {
    String title;
    Director director;
}

Director d = new Director();
d.name = "Christopher Nolan";
d.age = 53;

Movie m = new Movie();
m.title = "Oppenheimer";
m.director = d;

String json = new Gson().toJson(m);
System.out.println(json);
```

**Output:**

```json
{"title":"Oppenheimer","director":{"name":"Christopher Nolan","age":53}}
```

---

## 🧠 9. Customizing Serialization/Deserialization

Use a custom serializer or deserializer for fine control:

```java
import com.google.gson.*;

import java.lang.reflect.Type;

class MovieAdapter implements JsonSerializer<Movie>, JsonDeserializer<Movie> 
{
    @Override
    public JsonElement serialize(Movie movie, Type type, JsonSerializationContext ctx) 
    {
        JsonObject obj = new JsonObject();
        obj.addProperty("name", movie.getTitle());
        obj.addProperty("release_year", movie.getYear());
        return obj;
    }

    @Override
    public Movie deserialize(JsonElement json, Type type, JsonDeserializationContext ctx)
            throws JsonParseException {
        JsonObject obj = json.getAsJsonObject();
        return new Movie(
                obj.get("name").getAsString(),
                obj.get("release_year").getAsInt(),
                "Unknown",
                0.0
        );
    }
}
```

Register your adapter:

```java
Gson gson = new GsonBuilder()
        .registerTypeAdapter(Movie.class, new MovieAdapter())
        .setPrettyPrinting()
        .create();
```

---

## 🧾 10. Nulls, Exclusion, and Config Options

| Builder Option                            | Description                                       |
|-------------------------------------------|---------------------------------------------------|
| `.serializeNulls()`                       | Include `null` fields in JSON                     |
| `.excludeFieldsWithoutExposeAnnotation()` | Only include fields annotated with `@Expose`      |
| `.setPrettyPrinting()`                    | Format JSON output                                |
| `.disableHtmlEscaping()`                  | Prevent escaping HTML characters like `<` and `>` |

Example:

```java
Gson gson = new GsonBuilder()
        .serializeNulls()
        .disableHtmlEscaping()
        .setPrettyPrinting()
        .create();
```

---

## 🔍 11. Gson vs Jackson (Quick Comparison)

| Feature         | Gson              | Jackson                 |
|-----------------|-------------------|-------------------------|
| Simplicity      | ✅ Easier          | ⚙️ More complex         |
| Performance     | ⚡ Fast            | ⚡ Faster for large data |
| Customization   | Good              | Excellent               |
| Android Support | ✅ Native          | Limited                 |
| Common Use      | Small/medium apps | Enterprise systems      |

💡 **Gson** is perfect for teaching, small apps, and CLI projects — like your Movie Catalog.

---

## 🧩 12. Example Integration in Your Movie Catalog App

You’re already using this structure:

```
src/
 ├── main/java/
 │   ├── model/Movie.java
 │   ├── service/MovieService.java
 │   ├── util/JsonUtil.java  ← uses Gson here
 │   └── app/App.java
 └── main/resources/movies.json
```

Inside `JsonUtil.java`, you used:

```java
private static final Gson gson = new Gson();
```

and methods like:

```java
gson.toJson(list, writer);
gson.fromJson(reader, listType);
```

That’s exactly how professional apps use Gson — for lightweight, fast JSON persistence without databases.

---

## ✅ 13. Summary

| Feature                  | Description                                  |
|--------------------------|----------------------------------------------|
| **Library**              | `com.google.code.gson:gson`                  |
| **Purpose**              | Convert between Java objects and JSON        |
| **Key Classes**          | `Gson`, `GsonBuilder`, `TypeToken`           |
| **Main Methods**         | `toJson()`, `fromJson()`                     |
| **Supports**             | Lists, Maps, nested objects, custom adapters |
| **File I/O Integration** | Works perfectly with `Reader`/`Writer`       |
| **Latest Version**       | 2.11.0                                       |

---

## 💡 14. Pro Tips

* Always use `GsonBuilder().setPrettyPrinting()` when saving readable JSON files.
* For lists, always use `TypeToken<List<T>>` — otherwise, you’ll get generic type issues.
* Gson automatically ignores extra fields in JSON (useful when data shape changes).
* Use `@Expose` and `excludeFieldsWithoutExposeAnnotation()` for API serialization control.
* You can reuse the same `Gson` instance safely — it’s **thread-safe** and efficient.

---

## 🧠 15. Example Pretty JSON Output (Your Movie Catalog)

```json
[
  {
    "title": "Inception",
    "year": 2010,
    "genre": "Sci-Fi",
    "rating": 8.8
  },
  {
    "title": "The Godfather",
    "year": 1972,
    "genre": "Crime",
    "rating": 9.2
  }
]
```

---
