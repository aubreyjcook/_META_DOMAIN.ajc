# Java

## Purpose

Workflow for initializing a Java project in a blank repository. Java requires a build tool: **Maven** (most common) or **Gradle**.

## Prerequisites

- A **JDK**.
- **Maven** (or **Gradle**).

## Workflow (Maven)

### 1. Generate the skeleton

```bash
mvn archetype:generate -DgroupId=com.example -DartifactId=app
```

The command is interactive; alternatively write `pom.xml` by hand.

### 2. Confirm the structure

```plaintext
project/
├── pom.xml
└── src/
    ├── main/java/com/example/App.java
    └── test/java/com/example/AppTest.java
```

### 3. Configure pom.xml

Set the compiler release and add dependencies:

```xml
<properties>
  <maven.compiler.release>21</maven.compiler.release>
</properties>
```

### 4. Build and run

```bash
mvn compile      # compile
mvn test         # run tests
mvn package      # produce target/app-1.0.jar
java -jar target/app-1.0.jar
```

## Gradle alternative

```bash
gradle init         # scaffold a build.gradle project
gradle build
gradle run
```

## Notes

- `target/` (Maven) or `build/` (Gradle) belongs in `.gitignore`.
- Package names map to directory paths (`com.example` → `com/example/`).

## See Also

- [C++](cpp.md) — another compiled language's initialization pattern.
