# Note Manager with SQLite

A Spring Boot application designed to help with **notes management**. This project is a setup for a fully-featured notes management application, offering core functionalities like creating, reading, updating, and deleting notes.
The purpose of this application is to provide a backend service for managing notes, which can be further developed into a comprehensive note management system with persistence, authentication, and additional features.

This project is an enhanced version of the [Note Manager](https://github.com/ruslanaprus/goit-academy-dev-hw14), and it has an improved architecture for better maintainability and scalability. The `NoteService` interface is used to make the service layer more testable and extendable.
Similarly, the introduction of the `NoteRepository` interface ensures flexibility in switching between different data storage solutions, such as in-memory or database options.

---

## Getting Started

If you are configuring Hibernate to use SQLite in your `application.properties` file for a Spring Boot project, you should specify the correct Hibernate dialect. For SQLite, you can use `org.hibernate.community.dialect.SQLiteDialect` (Hibernate 6 or later).
```properties
# SQLite database configuration
spring.datasource.url=${DB_URL}   # Example: jdbc:sqlite:database.db
spring.datasource.driver-class-name=org.sqlite.JDBC

# Hibernate dialect for SQLite
spring.jpa.properties.hibernate.dialect=org.hibernate.community.dialect.SQLiteDialect
```
**Note**: Hibernate expects `BIGINT` for the `id` field in `Note` entity, which corresponds to Java's `Long`. SQLite does not have a native `BIGINT`, but in SQLite the `INTEGER` type can store up to 8-byte signed integers, which is sufficient for Java's `Long`. However, Hibernate is expecting a column type explicitly mapped to `BIGINT`, and SQLite's `INTEGER` is not recognized as such during schema validation. To solve this we can explicitly specify the SQL column type in the `@Column` annotation:
```Java
@Column(name = "id", columnDefinition = "INTEGER")
private Long id;
```

### Prerequisites

- **Java 21**: Ensure Java 21 is installed on your system.
- **Gradle**: This project uses Gradle for dependency management and build tasks.

### Installation

1. Clone the repository:
```shell
git clone git@github.com:ruslanaprus/spring-data-sqlite.git
cd spring-data-sqlite
```
2. Database Configuration: Copy the `.env.example` file into `.env`, and populate it with your DB details (key: [DB_URL]). This file will be used to set DB properties for Flyway plugin in `build.gradle` and for your application.

3. Build the project:
```shell
./gradlew clean build
```
4. Run the application:
```shell
./gradlew bootRun
```

---

## Technologies Used

- **Java 21**: Utilized for its modern features and enhanced performance.
- **Spring Boot 3.4.0**: Simplifies application development with embedded server and configuration support.
- **Spring Data JPA**: Provides powerful instruments for database access.
- **SQLite**: Database for storing note data.
- **Flyway**: Manages database schema migrations and populates initial data.
- **Jakarta Bean Validation**: Ensures data integrity with annotations `@NotNull` and `@NotEmpty`.
- **Lombok**: Reduces boilerplate code with annotations like `@Builder`, `@Getter`, and `@RequiredArgsConstructor`.
- **JUnit 5 & Mockito**: Provides a robust framework for writing unit and integration tests.
- **Thymeleaf**: Template engine for rendering dynamic HTML pages with embedded backend data.

---

## Main Features

- **Database-Backed Note Management**: Notes are stored in a SQLite database managed by Flyway migrations.
- **CRUD Operations**:
  - **List All Notes**: Retrieve all existing notes.
  - **Get Note by ID**: Fetch a specific note using its unique ID.
  - **Create Note**: Add a new note with an auto-generated ID.
  - **Update Note**: Modify an existing note.
  - **Delete Note**: Remove a note by ID.
- **User Input Validation**: Validates note data such as title and content using Jakarta Bean Validation.
- **Pagination**: Allows efficient display of notes, in case of large datasets.
- **Web layer**: Manages HTTP requests for the note management functionality. Supports endpoints for listing, updating, and deleting notes.
- **Thymeleaf integration**: Added Thymeleaf templates to render data on web pages directly from the backend:
  - **List All Notes**: Displays all notes.
  - **Create a Note**: Form to create a new note.
  - **Edit Note**: Form to update a note.
  - **Error page**: Displays user-friendly error messages for invalid inputs or missing resources.

---