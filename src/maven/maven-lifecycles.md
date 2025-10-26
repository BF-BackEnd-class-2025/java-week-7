# Maven Lifecycles

Maven has **three built-in lifecycles**:

1. **default** (or build) lifecycle
2. **clean** lifecycle : handles project cleaning
3. **site** lifecycle : handles project documentation

Each lifecycle consists of multiple **phases**.

---

## 1. Default (Build) Lifecycle

The **default lifecycle** handles project deployment. It is invoked automatically when you run commands like `mvn compile` or `mvn package`.

### Key Phases

| Phase                   | Description                                                             |
|-------------------------|-------------------------------------------------------------------------|
| validate                | Validate project structure and configuration.                           |
| initialize              | Initialize build environment, create directories.                       |
| generate-sources        | Generate any source code required.                                      |
| process-sources         | Process source code if needed (e.g., filtering).                        |
| generate-resources      | Generate resources needed for compilation.                              |
| process-resources       | Copy and process resources into `target/classes`.                       |
| compile                 | Compile the source code of the project.                                 |
| process-classes         | Post-process compiled classes (optional).                               |
| generate-test-sources   | Generate test source code.                                              |
| process-test-sources    | Process test sources.                                                   |
| generate-test-resources | Generate resources needed for tests.                                    |
| process-test-resources  | Copy test resources to `target/test-classes`.                           |
| test-compile            | Compile the test source code.                                           |
| test                    | Run unit tests using a testing framework (e.g., JUnit).                 |
| package                 | Package compiled code into JAR/WAR/EAR.                                 |
| verify                  | Run checks to ensure the package is valid.                              |
| install                 | Install the package into the local Maven repository (~/.m2/repository). |
| deploy                  | Copy the package to a remote repository for sharing with others.        |

**Example Commands:**

```bash
mvn compile   # Only compiles source code
mvn test      # Compiles and runs tests
mvn package   # Builds JAR/WAR in target/
mvn install   # Installs package locally
````

---

## 2. Clean Lifecycle

The **clean lifecycle** handles removing previous build artifacts.

### Key Phases

| Phase      | Description                                                       |
|------------|-------------------------------------------------------------------|
| pre-clean  | Actions before cleaning (optional).                               |
| clean      | Deletes the `target/` directory with compiled classes, JARs, etc. |
| post-clean | Actions after cleaning (optional).                                |

**Example Command:**

```bash
mvn clean
```

This ensures the next build starts fresh.

---

## 3. Site Lifecycle

The **site lifecycle** is used for generating project documentation.

### Key Phases

| Phase       | Description                                                      |
|-------------|------------------------------------------------------------------|
| pre-site    | Prepare for site generation.                                     |
| site        | Generate the project’s site documentation (HTML pages, reports). |
| post-site   | Post-processing for the site.                                    |
| site-deploy | Deploy the generated site to a web server or repository.         |

**Example Command:**

```bash
mvn site
mvn site-deploy
```

---

# Summary

Maven lifecycles help organize your build process into **phases**, making it:

* Predictable: each phase does a specific task
* Extensible: you can attach plugins to phases
* Reproducible: other developers can run the same lifecycle

| Lifecycle | Purpose                                 |
|-----------|-----------------------------------------|
| default   | Compile, test, package, install, deploy |
| clean     | Remove previous build artifacts         |
| site      | Generate project documentation          |

---

# References

* [Maven Official Documentation](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html)
* [Maven Phases Reference](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#Lifecycle_Reference)

```

---
