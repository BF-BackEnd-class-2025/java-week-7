# Understanding `pom.xml` in Maven

The `pom.xml` (Project Object Model) is the **heart of a Maven project**. It defines the project structure, dependencies,
build configuration, plugins, and other metadata. This file is used by Maven to build, test, and package your project.

---

## 1. Project Coordinates

Every Maven project has **coordinates** to uniquely identify it:

```xml
<groupId>org.example</groupId>
<artifactId>expense-tracker</artifactId>
<version>1.0-SNAPSHOT</version>
````

* **groupId** → your organization or package name
* **artifactId** → the name of the project
* **version** → project version (SNAPSHOT means in-development)

---

## 2. Properties

Properties are variables used throughout your `pom.xml`:

```xml
<properties>
    <maven.compiler.source>21</maven.compiler.source>
    <maven.compiler.target>21</maven.compiler.target>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
</properties>
```

* `maven.compiler.source` / `target` → Java version
* `project.build.sourceEncoding` → encoding of source files

---

## 3. Dependencies

Dependencies are **external libraries** your project needs. Maven will download them automatically from repositories.

```xml
<dependencies>
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
</dependencies>
```

* Dependencies go inside `<dependencies>`
* Maven automatically adds them to the **classpath** when compiling and running

---

## 4. Build Plugins

Plugins are tools Maven uses during the build process, like compiling code, packaging JARs, or running tests.

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-jar-plugin</artifactId>
            <version>3.3.0</version>
            <configuration>
                <archive>
                    <manifest>
                        <mainClass>org.example.app.App</mainClass>
                    </manifest>
                </archive>
            </configuration>
        </plugin>
    </plugins>
</build>
```

* **maven-jar-plugin** → builds a JAR and can specify `mainClass`
* Other common plugins:

    * `maven-compiler-plugin` → configure Java version
    * `maven-surefire-plugin` → run tests
    * `maven-shade-plugin` → build fat JAR including dependencies
    * `exec-maven-plugin` → run Java programs directly

---

## 5. Main Class (Executable JAR)

To make a JAR **runnable**, you need to specify the main class:

```xml
<mainClass>org.example.app.App</mainClass>
```

* Placed inside `<maven-jar-plugin>` configuration
* JVM uses it as the entry point when running `java -jar`

---

## 6. Build Configuration

The `<build>` section can also define:

* **Source directories** (`src/main/java`, `src/main/resources`)
* **Output directory** (`target/`)
* **Resource filtering** → include properties or files in JAR
* **Custom plugins** for packaging, code analysis, or deployment

---

## 7. Repositories (Optional)

If your dependencies are not on Maven Central, you can add custom repositories:

```xml
<repositories>
    <repository>
        <id>my-repo</id>
        <url>https://example.com/maven2/</url>
    </repository>
</repositories>
```

---

## 8. Summary Table

| Section                    | Purpose                                        |
|----------------------------|------------------------------------------------|
| groupId/artifactId/version | Identify project                               |
| properties                 | Define reusable variables                      |
| dependencies               | Add external libraries                         |
| build/plugins              | Configure build tools and tasks                |
| mainClass                  | Specify the entry point for JAR                |
| repositories               | Add custom remote repositories                 |
| build configuration        | Source, resources, output directory, filtering |

---

## References

* [Maven POM Reference](https://maven.apache.org/pom.html)
* [Maven Plugins](https://maven.apache.org/plugins/)
* [Maven Guide](https://maven.apache.org/guides/index.html)

---