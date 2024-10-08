# Spring Boot and JPA: Overview

├── @SpringBootApplication  
│   ├── Combines three main annotations:  
│   │   ├── @Configuration  
│   │   ├── @EnableAutoConfiguration  
│   │   └── @ComponentScan  
│   └── Purpose:  
│       └── Enables component scanning and auto-configuration  
│
├── Stereotype Annotations  
│   ├── @RestController  
│   │   ├── Combines @Controller and @ResponseBody  
│   │   └── Specialized for RESTful web services  
│   ├── @Service  
│   │   ├── Marks a class as a service provider  
│   │   └── Specialization of @Component for business logic  
│   └── @Repository  
│       ├── Indicates a data access component  
│       ├── Enables exception translation  
│       └── Specialization of @Component  
│
├── Spring Data JPA and Repositories  
│   ├── Repository Interface  
│   │   ├── Extends JpaRepository<Entity, ID>  
│   │   ├── Provides CRUD operations  
│   │   └── Example:  
│   │       └── public interface MovieRepo extends JpaRepository<Movie, Long> {...}  
│   ├── Automatic Implementation  
│   │   ├── Spring scans for interfaces extending JpaRepository  
│   │   ├── Creates proxy implementations at runtime  
│   │   └── Injects proxies into dependent classes  
│   ├── Custom Query Methods  
│   │   ├── @Query annotation for custom JPQL or SQL queries  
│   │   ├── Method name conventions for query derivation  
│   │   └── Use of @Param for parameter binding  
│   └── Transaction Management  
│       ├── @Transactional annotation  
│       └── @Modifying for write operations  
│
├── Dependency Injection  
│   ├── Handled automatically for beans  
│   ├── Constructor-based injection recommended  
│   ├── Benefits:  
│   │   ├── Immutability  
│   │   ├── Testability  
│   │   └── Clear contracts  
│   └── Example:  
│       └── public class MovieService { private final MovieRepo repo; ... }  
│
├── JPA Entities and Annotations  
│   ├── @Entity  
│   │   ├── Marks a class as a JPA entity  
│   │   └── Mapped to a database table  
│   ├── @Table  
│   │   └── Specifies custom table name  
│   ├── @Id  
│   │   └── Denotes the primary key field  
│   ├── @GeneratedValue  
│   │   └── Specifies primary key generation strategy  
│   ├── @Column  
│   │   └── Customizes column mapping  
│   └── Field Validation  
│       └── Annotations like @NotBlank, @Email for validation  
│
├── Custom Queries with @Query  
│   ├── Defining custom JPQL or native SQL queries  
│   ├── @Query annotation usage  
│   ├── JPQL vs Native SQL  
│   │   ├── JPQL: Object-oriented, database-independent  
│   │   └── Native SQL: Database-specific syntax  
│   └── Example:  
│       ├── JPQL: @Query("SELECT m FROM Movie m WHERE m.year > :year")  
│       └── Native SQL: @Query(value = "SELECT * FROM movies WHERE year > :year", nativeQuery = true)  
│
├── Transaction Management  
│   ├── @Transactional Annotation  
│   │   ├── Manages transaction boundaries  
│   │   ├── Can be applied at method or class level  
│   │   └── Spring uses AOP proxies for transaction management  
│   └── Example:  
│       └── @Transactional public void updateMovie(Movie movie) { ... }  
│
├── Comparison: With and Without Spring Boot  
│   ├── With Spring Boot:  
│   │   ├── Auto-configuration simplifies setup  
│   │   ├── Starter dependencies manage versions  
│   │   ├── Reduced boilerplate code  
│   │   └── Quick development and easy setup  
│   └── Without Spring Boot:  
│       ├── Manual configuration required  
│       │   ├── persistence.xml for JPA settings  
│       │   ├── EntityManagerFactory management  
│       │   └── DAO classes for data access  
│       ├── More boilerplate code  
│       └── Potential for configuration errors  
│
└── Frameworks and Libraries Used by Spring Boot  
      ├── Spring Framework  
      │   ├── Core support for dependency injection, AOP, etc.  
      │   └── Modules: Core, AOP, JDBC, ORM, Web MVC  
      ├── Spring Data  
      │   ├── Simplifies data access  
      │   └── Supports JPA, MongoDB, Redis, etc.  
      ├── Spring Security  
      │   └── Authentication and authorization features  
      ├── Hibernate ORM  
      │   └── Default JPA provider for ORM  
      ├── Embedded Servers  
      │   ├── Tomcat (default)  
      │   ├── Jetty  
      │   └── Undertow  
      ├── View Technologies  
      │   ├── Thymeleaf  
      │   └── Alternative: FreeMarker, JSP  
      ├── Jackson  
      │   └── JSON processing and serialization  
      ├── Logging Frameworks  
      │   ├── SLF4J  
      │   └── Logback  
      ├── Databases  
      │   ├── H2 Database (in-memory for testing)  
      │   └── Support for MySQL, PostgreSQL, etc.  
      ├── Testing Tools  
      │   ├── JUnit  
      │   └── Spring Test  
      ├── Actuator  
      │   └── Monitoring and management endpoints  
      ├── DevTools  
      │   └── Enhanced development experience  
      ├── Validation API  
      │   └── Data validation with Hibernate Validator  
      ├── Spring HATEOAS  
      │   └── Building hypermedia-driven RESTful web services  
      └── Project Lombok  
          └── Reduces boilerplate code with annotations like @Data


Key Takeaways:
- **@SpringBootApplication** simplifies configuration by combining essential annotations.
- **Stereotype annotations** like **@RestController**, **@Service**, and **@Repository** help organize the application layers.
- **Spring Data JPA** automates the implementation of repository interfaces, reducing boilerplate code.
- **Dependency Injection** enhances testability and maintainability through constructor-based injection.
- **Custom queries** can be defined using **@Query**, supporting both JPQL and native SQL.
- **Transaction management** is simplified with the **@Transactional** annotation.
- Using **Spring Boot** offers significant advantages over manual configuration, including auto-configuration and starter dependencies.
- **Spring Boot** integrates a wide array of frameworks and libraries, providing a robust ecosystem for application development.

------------------------------

---

## 1. Project Setup

Create a new Spring Boot project with the following dependencies:

- **Spring Web**
- **Spring Data JPA**
- **H2 Database** (for in-memory database)
- **Spring Boot DevTools** (optional, for development convenience)

---

Here is the complete code for the movie API with all necessary imports, package structure, and added getters and setters for the `Movie` entity.

### 1. Application Entry Point

```java
package com.example.movieapi;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class MovieApiApplication {
    public static void main(String[] args) {
        SpringApplication.run(MovieApiApplication.class, args);
    }
}
```

---

### 2. Movie Entity (with Getters and Setters)

```java
package com.example.movieapi.entity;

import jakarta.persistence.*;
import jakarta.validation.constraints.Max;
import jakarta.validation.constraints.Min;
import jakarta.validation.constraints.NotBlank;

import java.time.LocalDate;

@Entity
@Table(name = "movies")
public class Movie {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @NotBlank(message = "Title is mandatory")
    private String title;

    private String description;

    private String director;

    private String genre;

    @Min(0)
    @Max(10)
    private Double rating;

    private LocalDate releaseDate;

    // Constructors
    public Movie() {
    }

    public Movie(String title, String description, String director, String genre, Double rating, LocalDate releaseDate) {
        this.title = title;
        this.description = description;
        this.director = director;
        this.genre = genre;
        this.rating = rating;
        this.releaseDate = releaseDate;
    }

    // Getters and Setters
    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public String getDescription() {
        return description;
    }

    public void setDescription(String description) {
        this.description = description;
    }

    public String getDirector() {
        return director;
    }

    public void setDirector(String director) {
        this.director = director;
    }

    public String getGenre() {
        return genre;
    }

    public void setGenre(String genre) {
        this.genre = genre;
    }

    public Double getRating() {
        return rating;
    }

    public void setRating(Double rating) {
        this.rating = rating;
    }

    public LocalDate getReleaseDate() {
        return releaseDate;
    }

    public void setReleaseDate(LocalDate releaseDate) {
        this.releaseDate = releaseDate;
    }
}
```

---

### 3. Movie Repository

```java
package com.example.movieapi.repository;

import com.example.movieapi.entity.Movie;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
import org.springframework.data.repository.query.Param;
import org.springframework.stereotype.Repository;

import java.time.LocalDate;
import java.util.List;

@Repository
public interface MovieRepository extends JpaRepository<Movie, Long> {

    List<Movie> findByTitleContainingIgnoreCase(String title);

    List<Movie> findByGenre(String genre);

    @Query("SELECT m FROM Movie m WHERE m.rating >= :rating")
    List<Movie> findByRatingGreaterThanEqual(@Param("rating") Double rating);

    @Query("SELECT m FROM Movie m WHERE m.releaseDate BETWEEN :startDate AND :endDate")
    List<Movie> findByReleaseDateBetween(
        @Param("startDate") LocalDate startDate,
        @Param("endDate") LocalDate endDate);
}
```

---

### 4. Movie Service

```java
package com.example.movieapi.service;

import com.example.movieapi.entity.Movie;
import com.example.movieapi.exception.ResourceNotFoundException;
import com.example.movieapi.repository.MovieRepository;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import java.time.LocalDate;
import java.util.List;
import java.util.Optional;

@Service
public class MovieService {

    private final MovieRepository movieRepository;

    public MovieService(MovieRepository movieRepository) {
        this.movieRepository = movieRepository;
    }

    public List<Movie> getAllMovies() {
        return movieRepository.findAll();
    }

    public Optional<Movie> getMovieById(Long id) {
        return movieRepository.findById(id);
    }

    public Movie createMovie(Movie movie) {
        return movieRepository.save(movie);
    }

    @Transactional
    public Movie updateMovie(Long id, Movie movieDetails) {
        Movie movie = movieRepository.findById(id)
            .orElseThrow(() -> new ResourceNotFoundException("Movie not found with id " + id));

        movie.setTitle(movieDetails.getTitle());
        movie.setDescription(movieDetails.getDescription());
        movie.setDirector(movieDetails.getDirector());
        movie.setGenre(movieDetails.getGenre());
        movie.setRating(movieDetails.getRating());
        movie.setReleaseDate(movieDetails.getReleaseDate());

        return movie;
    }

    public void deleteMovie(Long id) {
        if (!movieRepository.existsById(id)) {
            throw new ResourceNotFoundException("Movie not found with id " + id);
        }
        movieRepository.deleteById(id);
    }

    // Additional search methods
    public List<Movie> searchMoviesByTitle(String title) {
        return movieRepository.findByTitleContainingIgnoreCase(title);
    }

    public List<Movie> searchMoviesByGenre(String genre) {
        return movieRepository.findByGenre(genre);
    }

    public List<Movie> searchMoviesByRating(Double rating) {
        return movieRepository.findByRatingGreaterThanEqual(rating);
    }

    public List<Movie> searchMoviesByReleaseDateRange(LocalDate startDate, LocalDate endDate) {
        return movieRepository.findByReleaseDateBetween(startDate, endDate);
    }
}
```

---

### 5. Movie Controller

```java
package com.example.movieapi.controller;

import com.example.movieapi.entity.Movie;
import com.example.movieapi.exception.ResourceNotFoundException;
import com.example.movieapi.service.MovieService;
import org.springframework.format.annotation.DateTimeFormat;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.validation.annotation.Validated;
import org.springframework.web.bind.annotation.*;

import jakarta.validation.Valid;
import java.time.LocalDate;
import java.util.List;

@RestController
@RequestMapping("/api/movies")
public class MovieController {

    private final MovieService movieService;

    public MovieController(MovieService movieService) {
        this.movieService = movieService;
    }

    @GetMapping
    public List<Movie> getAllMovies() {
        return movieService.getAllMovies();
    }

    @GetMapping("/{id}")
    public ResponseEntity<Movie> getMovieById(@PathVariable Long id) {
        Movie movie = movieService.getMovieById(id)
            .orElseThrow(() -> new ResourceNotFoundException("Movie not found with id " + id));
        return ResponseEntity.ok(movie);
    }

    @PostMapping
    public ResponseEntity<Movie> createMovie(@Valid @RequestBody Movie movie) {
        Movie savedMovie = movieService.createMovie(movie);
        return ResponseEntity.status(HttpStatus.CREATED).body(savedMovie);
    }

    @PutMapping("/{id}")
    public ResponseEntity<Movie> updateMovie(
        @PathVariable Long id, @Valid @RequestBody Movie movieDetails) {
        Movie updatedMovie = movieService.updateMovie(id, movieDetails);
        return ResponseEntity.ok(updatedMovie);
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteMovie(@PathVariable Long id) {
        movieService.deleteMovie(id);
        return ResponseEntity.noContent().build();
    }

    // Search endpoints
    @GetMapping("/search")
    public List<Movie> searchMovies(
        @RequestParam(required = false) String title,
        @RequestParam(required = false) String genre,
        @RequestParam(required = false) Double rating,
        @RequestParam(required = false) @DateTimeFormat(iso = DateTimeFormat.ISO.DATE) LocalDate startDate,
        @RequestParam(required = false) @DateTimeFormat(iso = DateTimeFormat.ISO.DATE) LocalDate endDate) {

        if (title != null) {
            return movieService.searchMoviesByTitle(title);
        } else if (genre != null) {
            return movieService.searchMoviesByGenre(genre);
        } else if (rating != null) {
            return movieService.searchMoviesByRating(rating);
        } else if (startDate != null && endDate != null) {
            return movieService.searchMoviesByReleaseDateRange(startDate, endDate);
        } else {
            return movieService.getAllMovies();
        }
    }
}
```

---

### 6. Exception Handling

```java
package com.example.movieapi.exception;

import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.ResponseStatus;

@ResponseStatus(HttpStatus.NOT_FOUND)
public class ResourceNotFoundException extends RuntimeException {

    public ResourceNotFoundException(String message) {
        super(message);
    }
}
```

---

### 7. Application Properties

```properties
# application.properties

# H2 Database Configuration
spring.datasource.url=jdbc:h2:mem:moviesdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=

# Hibernate Configuration
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.jpa.hibernate.ddl-auto=update

# Show SQL statements in the console
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true
```

---

### 8. CORS Configuration (Optional)

```java
package com.example.movieapi.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void addCors

Mappings(CorsRegistry registry) {
        registry.addMapping("/api/**")
            .allowedOrigins("*") // Replace "*" with specific origins in production
            .allowedMethods("GET", "POST", "PUT", "DELETE", "OPTIONS");
    }
}
```

This code provides a complete Spring Boot movie API implementation, with added getters and setters for the `Movie` entity.

## 10. Project Structure

```
src
├── main
│   ├── java
│   │   └── com.example.movieapi
│   │       ├── MovieApiApplication.java
│   │       ├── controller
│   │       │   └── MovieController.java
│   │       ├── entity
│   │       │   └── Movie.java
│   │       ├── exception
│   │       │   └── ResourceNotFoundException.java
│   │       ├── repository
│   │       │   └── MovieRepository.java
│   │       ├── service
│   │       │   └── MovieService.java
│   │       └── config
│   │           └── WebConfig.java
│   └── resources
│       └── application.properties
```

---

## 11. Testing the API

You can test the API using tools like **Postman** or **cURL**.

- **GET** `/api/movies`: Retrieve all movies.
- **GET** `/api/movies/{id}`: Retrieve a movie by ID.
- **POST** `/api/movies`: Create a new movie.
- **PUT** `/api/movies/{id}`: Update an existing movie.
- **DELETE** `/api/movies/{id}`: Delete a movie.
- **GET** `/api/movies/search?title=Inception`: Search movies by title.

---

## 12. Applying the Key Points

- **@SpringBootApplication**: Used to enable component scanning and auto-configuration.
- **Stereotype Annotations**:
  - `@RestController`: For the `MovieController` to handle RESTful requests.
  - `@Service`: For the `MovieService` to encapsulate business logic.
  - `@Repository`: For the `MovieRepository` to indicate data access layer.
- **Dependency Injection**: Constructor-based injection is used throughout the application.
- **Spring Data JPA**:
  - `JpaRepository` is extended to provide CRUD operations.
  - Custom query methods and `@Query` annotations are used for complex queries.
- **Transaction Management**:
  - `@Transactional` is applied to methods that require transactional behavior.
- **Entity Annotations**:
  - `@Entity`, `@Table`, `@Id`, and `@GeneratedValue` are used to map the `Movie` class to a database table.
- **Validation**:
  - Annotations like `@NotBlank`, `@Min`, `@Max` are used for data validation.
  - `@Valid` is used in controller methods to enforce validation.
- **Exception Handling**:
  - A custom `ResourceNotFoundException` is created to handle not found errors.
- **CORS Configuration**:
  - Configured via `WebMvcConfigurer` to handle cross-origin requests.

---

## 13. Additional Considerations

- **Pagination and Sorting**: Can be implemented using Spring Data JPA's `Pageable` interface.
- **Security**: Integrate Spring Security to secure your API endpoints.
- **Testing**: Write unit tests and integration tests using JUnit and Spring Boot's testing support.
- **Documentation**: Use Swagger or Spring REST Docs to document your API.

---

## 14. Conclusion

By following the best practices and applying the key points discussed, we've built a robust and scalable Movie API similar to the TMDB API. This application demonstrates how to use Spring Boot, Spring Data JPA, and various annotations to create a clean and maintainable codebase.

---

Feel free to expand on this foundation by adding more features such as user authentication, reviews, ratings, or integrating with external services.



# Building a Movie API with Spring Boot and JPA: Step by Step

In this exercise, we'll build a Movie API step by step using Spring Boot and Spring Data JPA. At each step, we'll ask and answer questions to verify our understanding before proceeding. This approach will help us understand how libraries are used, how auto-configuration works, exception handling, database connections, updates, and other essential details.

---

## Table of Contents

1. [Project Setup](#1-project-setup)
2. [Application Entry Point](#2-application-entry-point)
3. [Creating the Movie Entity](#3-creating-the-movie-entity)
4. [Defining the Repository](#4-defining-the-repository)
5. [Implementing the Service Layer](#5-implementing-the-service-layer)
6. [Building the Controller](#6-building-the-controller)
7. [Exception Handling](#7-exception-handling)
8. [Configuration and Properties](#8-configuration-and-properties)
9. [Testing the API](#9-testing-the-api)
10. [Conclusion](#10-conclusion)

---

## 1. Project Setup

### **Step 1: Initialize the Project**

Use the Spring Initializr to create a new Maven project:

- **Project**: Maven Project
- **Language**: Java
- **Spring Boot**: Latest stable version
- **Group**: `com.example`
- **Artifact**: `movieapi`
- **Name**: `Movie API`
- **Package Name**: `com.example.movieapi`
- **Packaging**: Jar
- **Java Version**: 11 (or your preferred version)

### **Step 2: Add Dependencies**

Include the following dependencies:

- **Spring Web**: For building web applications and RESTful services.
- **Spring Data JPA**: For database access using JPA.
- **H2 Database**: An in-memory database for development and testing.
- **Lombok** (optional): To reduce boilerplate code with annotations.
- **Spring Boot DevTools** (optional): Provides automatic restarts and live reload.
- **Validation**: For data validation (part of Spring Boot Starter Web).

### **Questions and Answers**

**Q1: Why do we include Spring Web and Spring Data JPA dependencies?**

**A1:** We include **Spring Web** to build web applications and RESTful APIs, providing MVC architecture and RESTful services. **Spring Data JPA** simplifies data access layers by providing repository support and database interaction using Java Persistence API (JPA).

---

**Q2: What is the purpose of including the H2 Database in our project?**

**A2:** H2 Database is an in-memory relational database. Including it allows us to develop and test our application without needing to set up an external database. It simplifies development and testing.

---

**Q3: How does Lombok help in our project?**

**A3:** Lombok reduces boilerplate code by generating getters, setters, constructors, and other methods at compile time using annotations like `@Getter`, `@Setter`, `@AllArgsConstructor`, etc. This makes the code cleaner and more maintainable.

---

## 2. Application Entry Point

### **MovieApiApplication.java**

```java
package com.example.movieapi;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class MovieApiApplication {

    public static void main(String[] args) {
        SpringApplication.run(MovieApiApplication.class, args);
    }
}
```

### **Explanation**

- `@SpringBootApplication` is a convenience annotation that combines:

  - `@Configuration`: Indicates that the class can be used by the Spring IoC container as a source of bean definitions.
  - `@EnableAutoConfiguration`: Tells Spring Boot to start adding beans based on classpath settings, other beans, and various property settings.
  - `@ComponentScan`: Tells Spring to look for other components, configurations, and services in the `com.example.movieapi` package, allowing it to find the controllers.

### **Questions and Answers**

**Q4: What is the role of the `main` method in the `MovieApiApplication` class?**

**A4:** The `main` method serves as the entry point of the application. It invokes `SpringApplication.run()`, which bootstraps the Spring Boot application, starts the embedded server (like Tomcat), and initializes all components.

---

**Q5: How does `@SpringBootApplication` simplify our configuration?**

**A5:** `@SpringBootApplication` combines three annotations (`@Configuration`, `@EnableAutoConfiguration`, and `@ComponentScan`) into one. This reduces the amount of boilerplate code and simplifies the setup by enabling component scanning and auto-configuration.

---

## 3. Creating the Movie Entity

### **Movie.java**

```java
package com.example.movieapi.entity;

import javax.persistence.*;
import javax.validation.constraints.*;
import java.time.LocalDate;

@Entity
@Table(name = "movies")
public class Movie {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @NotBlank(message = "Title is mandatory")
    private String title;

    private String description;

    private String director;

    private String genre;

    @Min(value = 0, message = "Rating must be between 0 and 10")
    @Max(value = 10, message = "Rating must be between 0 and 10")
    private Double rating;

    private LocalDate releaseDate;

    // Constructors
    public Movie() {
    }

    public Movie(String title, String description, String director, String genre, Double rating, LocalDate releaseDate) {
        this.title = title;
        this.description = description;
        this.director = director;
        this.genre = genre;
        this.rating = rating;
        this.releaseDate = releaseDate;
    }

    // Getters and Setters (Use Lombok annotations if included)
    // @Getter and @Setter annotations can be used if Lombok is included
}
```

### **Explanation**

- `@Entity`: Marks the class as a JPA entity mapped to a database table.
- `@Table(name = "movies")`: Specifies the table name in the database.
- `@Id`: Marks the primary key of the entity.
- `@GeneratedValue(strategy = GenerationType.IDENTITY)`: Indicates that the primary key value will be generated automatically by the database.
- Validation annotations like `@NotBlank`, `@Min`, and `@Max` enforce data integrity.

### **Questions and Answers**

**Q6: What is the purpose of the `@Entity` annotation?**

**A6:** The `@Entity` annotation specifies that the class is a JPA entity, meaning it's a Java class that maps to a table in the database. It allows the class to be managed by the JPA provider (e.g., Hibernate).

---

**Q7: Why do we use `@Table(name = "movies")`?**

**A7:** `@Table(name = "movies")` specifies the exact name of the database table to which the entity will be mapped. If omitted, the table name defaults to the class name.

---

**Q8: How do the validation annotations help in our entity?**

**A8:** Validation annotations ensure that the data adheres to specified rules before it's persisted to the database. For example, `@NotBlank` ensures that the `title` field is not null or empty, and `@Min` and `@Max` enforce that the `rating` is between 0 and 10.

---

## 4. Defining the Repository

### **MovieRepository.java**

```java
package com.example.movieapi.repository;

import com.example.movieapi.entity.Movie;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import org.springframework.data.jpa.repository.Query;
import org.springframework.data.repository.query.Param;

import java.time.LocalDate;
import java.util.List;

@Repository
public interface MovieRepository extends JpaRepository<Movie, Long> {

    List<Movie> findByTitleContainingIgnoreCase(String title);

    List<Movie> findByGenre(String genre);

    @Query("SELECT m FROM Movie m WHERE m.rating >= :rating")
    List<Movie> findByRatingGreaterThanEqual(@Param("rating") Double rating);

    @Query("SELECT m FROM Movie m WHERE m.releaseDate BETWEEN :startDate AND :endDate")
    List<Movie> findByReleaseDateBetween(
            @Param("startDate") LocalDate startDate,
            @Param("endDate") LocalDate endDate);
}
```

### **Explanation**

- Extending `JpaRepository<Movie, Long>` provides CRUD operations and JPA-related methods.
- `@Repository` is a stereotype annotation indicating that the interface is a repository.
- Custom query methods:

  - Method names like `findByTitleContainingIgnoreCase` allow Spring Data JPA to generate queries automatically.
  - `@Query` annotations define custom JPQL queries for more complex operations.
  - `@Param` binds method parameters to named parameters in the query.

### **Questions and Answers**

**Q9: What benefits do we get from extending `JpaRepository`?**

**A9:** Extending `JpaRepository` provides us with built-in CRUD operations, pagination, and sorting capabilities without writing boilerplate code. It simplifies data access and manipulation.

---

**Q10: How does Spring Data JPA interpret method names like `findByTitleContainingIgnoreCase`?**

**A10:** Spring Data JPA parses the method name and creates a query based on it. `findByTitleContainingIgnoreCase` translates to a SQL `LIKE` query that searches for movies where the title contains a specified string, ignoring case.

---

**Q11: When should we use the `@Query` annotation?**

**A11:** We use `@Query` when we need custom JPQL or SQL queries that cannot be derived from method names. It provides more flexibility for complex queries.

---

## 5. Implementing the Service Layer

### **MovieService.java**

```java
package com.example.movieapi.service;

import com.example.movieapi.entity.Movie;
import com.example.movieapi.exception.ResourceNotFoundException;
import com.example.movieapi.repository.MovieRepository;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import java.time.LocalDate;
import java.util.List;
import java.util.Optional;

@Service
public class MovieService {

    private final MovieRepository movieRepository;

    // Constructor-based Dependency Injection
    public MovieService(MovieRepository movieRepository) {
        this.movieRepository = movieRepository;
    }

    // CRUD Operations
    public List<Movie> getAllMovies() {
        return movieRepository.findAll();
    }

    public Optional<Movie> getMovieById(Long id) {
        return movieRepository.findById(id);
    }

    public Movie createMovie(Movie movie) {
        return movieRepository.save(movie);
    }

    @Transactional
    public Movie updateMovie(Long id, Movie movieDetails) {
        Movie movie = movieRepository.findById(id)
                .orElseThrow(() -> new ResourceNotFoundException("Movie not found with id " + id));

        movie.setTitle(movieDetails.getTitle());
        movie.setDescription(movieDetails.getDescription());
        movie.setDirector(movieDetails.getDirector());
        movie.setGenre(movieDetails.getGenre());
        movie.setRating(movieDetails.getRating());
        movie.setReleaseDate(movieDetails.getReleaseDate());

        return movie; // Changes will be persisted when the transaction commits
    }

    public void deleteMovie(Long id) {
        if (!movieRepository.existsById(id)) {
            throw new ResourceNotFoundException("Movie not found with id " + id);
        }
        movieRepository.deleteById(id);
    }

    // Search Methods
    public List<Movie> searchMoviesByTitle(String title) {
        return movieRepository.findByTitleContainingIgnoreCase(title);
    }

    public List<Movie> searchMoviesByGenre(String genre) {
        return movieRepository.findByGenre(genre);
    }

    public List<Movie> searchMoviesByRating(Double rating) {
        return movieRepository.findByRatingGreaterThanEqual(rating);
    }

    public List<Movie> searchMoviesByReleaseDateRange(LocalDate startDate, LocalDate endDate) {
        return movieRepository.findByReleaseDateBetween(startDate, endDate);
    }
}
```

### **Explanation**

- `@Service` indicates that this class is a service component in the business logic layer.
- Constructor-based dependency injection is used for `MovieRepository`.
- `@Transactional` ensures that the `updateMovie` method executes within a transaction.
- Business logic is encapsulated in the service layer, separating it from controllers and repositories.

### **Questions and Answers**

**Q12: Why do we use constructor-based dependency injection in `MovieService`?**

**A12:** Constructor-based injection is recommended because it makes the dependencies explicit and allows for immutable fields. It also facilitates easier testing and ensures that the service cannot be instantiated without its required dependencies.

---

**Q13: What is the purpose of the `@Transactional` annotation on the `updateMovie` method?**

**A13:** `@Transactional` ensures that all database operations within the `updateMovie` method are executed within a single transaction. If any operation fails, the entire transaction is rolled back, maintaining data integrity.

---

**Q14: How does the `updateMovie` method persist changes without explicitly calling a save method?**

**A14:** Since the `Movie` entity is managed by the persistence context within a transaction, any changes made to it are automatically detected and persisted when the transaction commits. This is a feature of JPA's entity management.

---

## 6. Building the Controller

### **MovieController.java**

```java
package com.example.movieapi.controller;

import com.example.movieapi.entity.Movie;
import com.example.movieapi.exception.ResourceNotFoundException;
import com.example.movieapi.service.MovieService;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.validation.annotation.Validated;
import org.springframework.web.bind.annotation.*;
import org.springframework.format.annotation.DateTimeFormat;

import javax.validation.Valid;
import java.time.LocalDate;
import java.util.List;

@RestController
@RequestMapping("/api/movies")
@Validated
public class MovieController {

    private final MovieService movieService;

    // Constructor-based Dependency Injection
    public MovieController(MovieService movieService) {
        this.movieService = movieService;
    }

    // Get All Movies
    @GetMapping
    public List<Movie> getAllMovies() {
        return movieService.getAllMovies();
    }

    // Get Movie by ID
    @GetMapping("/{id}")
    public ResponseEntity<Movie> getMovieById(@PathVariable Long id) {
        Movie movie = movieService.getMovieById(id)
                .orElseThrow(() -> new ResourceNotFoundException("Movie not found with id " + id));
        return ResponseEntity.ok(movie);
    }

    // Create New Movie
    @PostMapping
    public ResponseEntity<Movie> createMovie(@Valid @RequestBody Movie movie) {
        Movie savedMovie = movieService.createMovie(movie);
        return ResponseEntity.status(HttpStatus.CREATED).body(savedMovie);
    }

    // Update Movie
    @PutMapping("/{id}")
    public ResponseEntity<Movie> updateMovie(
            @PathVariable Long id, @Valid @RequestBody Movie movieDetails) {
        Movie updatedMovie = movieService.updateMovie(id, movieDetails);
        return ResponseEntity.ok(updatedMovie);
    }

    // Delete Movie
    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteMovie(@PathVariable Long id) {
        movieService.deleteMovie(id);
        return ResponseEntity.noContent().build();
    }

    // Search Movies
    @GetMapping("/search")
    public List<Movie> searchMovies(
            @RequestParam(required = false) String title,
            @RequestParam(required = false) String genre,
            @RequestParam(required = false) Double rating,
            @RequestParam(required = false) @DateTimeFormat(iso = DateTimeFormat.ISO.DATE) LocalDate startDate,
            @RequestParam(required = false) @DateTimeFormat(iso = DateTimeFormat.ISO.DATE) LocalDate endDate) {

        if (title != null) {
            return movieService.searchMoviesByTitle(title);
        } else if (genre != null) {
            return movieService.searchMoviesByGenre(genre);
        } else if (rating != null) {
            return movieService.searchMoviesByRating(rating);
        } else if (startDate != null && endDate != null) {
            return movieService.searchMoviesByReleaseDateRange(startDate, endDate);
        } else {
            return movieService.getAllMovies();
        }
    }
}
```

### **Explanation**

- `@RestController` combines `@Controller` and `@ResponseBody`, indicating that the controller returns JSON/XML responses.
- `@RequestMapping("/api/movies")` sets the base path for all endpoints in this controller.
- `@Validated` enables method-level validation.
- `@Valid` ensures that the `Movie` object is validated before processing.
- `ResponseEntity` allows us to control the HTTP status code and headers.

### **Questions and Answers**

**Q15: What is the difference between `@Controller` and `@RestController`?**

**A15:** `@Controller` is used in MVC applications and returns views (like HTML pages). `@RestController` is a convenience annotation that combines `@Controller` and `@ResponseBody`, indicating that the methods return data directly in the response body (e.g., JSON).

---

**Q16: How does `@Valid` work in the `createMovie` and `updateMovie` methods?**

**A16:** `@Valid` triggers the validation process for the `Movie` object based on the validation annotations in the `Movie` class. If validation fails, a `MethodArgumentNotValidException` is thrown.

---

**Q17: Why do we use `ResponseEntity` in some methods?**

**A17:** `ResponseEntity` allows us to customize the HTTP response, including status codes and headers. It provides more flexibility compared to returning the object directly.

---

**Q18: How does the `searchMovies` method handle multiple search criteria?**

**A18:** The method checks which request parameters are not `null` and calls the corresponding service method. This allows for flexible search capabilities based on the provided parameters.

---

## 7. Exception Handling

### **ResourceNotFoundException.java**

```java
package com.example.movieapi.exception;

import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.ResponseStatus;

@ResponseStatus(value = HttpStatus.NOT_FOUND)
public class ResourceNotFoundException extends RuntimeException {

    public ResourceNotFoundException(String message) {
        super(message);
    }
}
```

### **Explanation**

- Custom exception that extends `RuntimeException`.
- `@ResponseStatus(HttpStatus.NOT_FOUND)` indicates that when this exception is thrown, the HTTP status code should be 404 (Not Found).

### **Questions and Answers**

**Q19: What happens when `ResourceNotFoundException` is thrown in the controller?**

**A19:** When `ResourceNotFoundException` is thrown, the controller returns a response with HTTP status 404 (Not Found) and the exception's message in the response body.

---

**Q20: How can we handle exceptions globally in the application?**

**A20:** We can create a class annotated with `@ControllerAdvice` and define methods with `@ExceptionHandler` annotations to handle exceptions globally, providing custom responses for different exception types.

---

### **GlobalExceptionHandler.java** (Optional)

```java
package com.example.movieapi.exception;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;
import org.springframework.web.method.annotation.MethodArgumentTypeMismatchException;

import javax.validation.ConstraintViolationException;

@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<?> resourceNotFoundException(ResourceNotFoundException ex) {
        return new ResponseEntity<>(ex.getMessage(), HttpStatus.NOT_FOUND);
    }

    @ExceptionHandler(ConstraintViolationException.class)
    public ResponseEntity<?> constraintViolationException(ConstraintViolationException ex) {
        return new ResponseEntity<>(ex.getMessage(), HttpStatus.BAD_REQUEST);
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<?> globalExceptionHandler(Exception ex) {
        return new ResponseEntity<>("Internal Server Error", HttpStatus.INTERNAL_SERVER_ERROR);
    }
}
```

---

## 8. Configuration and Properties

### **application.properties**

```properties
# H2 Database Configuration
spring.datasource.url=jdbc:h2:mem:moviesdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=

# Hibernate Configuration
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.jpa.hibernate.ddl-auto=update

# Show SQL statements in the console
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true

# H2 Console Configuration
spring.h2.console.enabled=true
spring.h2.console.path=/h2-console
```

### **Explanation**

- **Data Source Configuration**: Sets up the in-memory H2 database.
- `spring.jpa.hibernate.ddl-auto=update`: Automatically updates the database schema based on the entities.
- `spring.jpa.show-sql=true`: Enables logging of SQL statements.
- `spring.h2.console.enabled=true`: Enables the H2 database console at `/h2-console`.

### **Questions and Answers**

**Q21: What is the purpose of `spring.jpa.hibernate.ddl-auto=update`?**

**A21:** It tells Hibernate to automatically update the database schema to match the entities each time the application runs. It creates tables and alters existing ones without dropping data.

---

**Q22: Why do we enable the H2 console?**

**A22:** Enabling the H2 console allows us to access the in-memory database through a web interface at `/h2-console`, which is useful for debugging and inspecting the database during development.

---

**Q23: How does auto-configuration work in Spring Boot regarding the database connection?**

**A23:** Spring Boot's auto-configuration detects the presence of the H2 database and the JPA dependency on the classpath. It automatically configures a `DataSource`, `EntityManagerFactory`, and `TransactionManager` using sensible defaults based on the properties provided.

---

## 9. Testing the API

### **Using Postman or cURL**

- **Retrieve All Movies**

  - **Request**: `GET http://localhost:8080/api/movies`

- **Retrieve a Movie by ID**

  - **Request**: `GET http://localhost:8080/api/movies/{id}`

- **Create a New Movie**

  - **Request**: `POST http://localhost:8080/api/movies`
  - **Headers**: `Content-Type: application/json`
  - **Body**:

    ```json
    {
      "title": "Inception",
      "description": "A mind-bending thriller",
      "director": "Christopher Nolan",
      "genre": "Sci-Fi",
      "rating": 8.8,
      "releaseDate": "2010-07-16"
    }
    ```

- **Update an Existing Movie**

  - **Request**: `PUT http://localhost:8080/api/movies/{id}`
  - **Headers**: `Content-Type: application/json`
  - **Body**: (Similar to create)

- **Delete a Movie**

  - **Request**: `DELETE http://localhost:8080/api/movies/{id}`

- **Search Movies**

  - By Title: `GET http://localhost:8080/api/movies/search?title=Inception`
  - By Genre: `GET http://localhost:8080/api/movies/search?genre=Sci-Fi`
  - By Rating: `GET http://localhost:8080/api/movies/search?rating=8`
  - By Release Date Range: `GET http://localhost:8080/api/movies/search?startDate=2010-01-01&endDate=2010-12-31`

### **Questions and Answers**

**Q24: How can we test the validation constraints on the `Movie` entity?**

**A24:** By sending requests with invalid data (e.g., missing the `title` field or setting a rating outside the 0-10 range) and observing if the application returns appropriate validation error messages and HTTP 400 Bad Request responses.

---

**Q25: What happens if we try to retrieve or delete a movie that doesn't exist?**

**A25:** The application will throw a `ResourceNotFoundException`, which results in an HTTP 404 Not Found response due to the exception handling mechanism we set up.

---

**Q26: How can we access the H2 database console, and what is it used for?**

**A26:** By navigating to `http://localhost:8080/h2-console` in a web browser. It is used for inspecting and querying the in-memory database, which is helpful for debugging and verifying data.

---

## 10. Conclusion

By building the Movie API step by step and asking questions along the way, we've:

- **Understood the use of key Spring annotations** like `@SpringBootApplication`, `@Entity`, `@Repository`, `@Service`, and `@RestController`.
- **Learned how auto-configuration works in Spring Boot**, simplifying our setup by automatically configuring beans based on the classpath and properties.
- **Explored exception handling** using custom exceptions and the `@ControllerAdvice` mechanism.
- **Implemented validation** to ensure data integrity.
- **Managed database connections and updates** using Spring Data JPA and H2 database.
- **Tested the API** to verify functionality and validation.

---

### **Final Questions and Answers**

**Q27: How does Spring Boot simplify the development of applications like our Movie API?**

**A27:** Spring Boot simplifies development by providing auto-configuration, starter dependencies, and embedded servers. This reduces boilerplate code and configuration, allowing developers to focus on writing business logic.

---

**Q28: What are the benefits of structuring the application into layers (entity, repository, service, controller)?**

**A28:** Layered architecture promotes separation of concerns, making the codebase more modular, maintainable, and testable. Each layer has a specific responsibility, which enhances readability and scalability.

---

**Q29: How does dependency injection benefit our application design?**

**A29:** Dependency injection decouples component creation from component usage, making the code more flexible and easier to test. It allows for loose coupling and better management of dependencies.

---

**Q30: What is the significance of using an in-memory database like H2 during development?**

**A30:** An in-memory database like H2 allows for quick setup and testing without the overhead of configuring an external database. It speeds up development and is ideal for small to medium-sized projects or prototyping.

---

# Conclusion

By completing this step-by-step exercise with questions and answers, we've gained a comprehensive understanding of how to build a RESTful API using Spring Boot and Spring Data JPA. We explored:

- **Project setup and dependency management**
- **Application entry point and auto-configuration**
- **Entity creation and data validation**
- **Repository definition and custom queries**
- **Service layer implementation with transaction management**
- **Controller development following REST principles**
- **Exception handling strategies**
- **Database configuration and connection management**
- **Testing and validating the API**

This approach not only helped us build the application but also reinforced key concepts at each stage, ensuring a deeper understanding of the technologies and best practices involved.

Feel free to extend this application by adding features such as user authentication, integrating with external APIs. Continue to explore and ask questions to enhance your mastery of Spring Boot and JPA.

---

## Coding Questions

1. **Write the entry point for a Spring Boot application.**
   - Expected answer: Implement `MovieApiApplication.java` with the `@SpringBootApplication` annotation.

2. **Create a JPA entity class for a `Movie` with fields like `title`, `description`, `director`, and `rating`. Include validation annotations.**
   - Expected answer: Define the `Movie` entity with appropriate JPA annotations and validation constraints (e.g., `@NotBlank`, `@Min`, `@Max`).

3. **Write a `MovieRepository` interface that extends `JpaRepository` for managing `Movie` entities.**
   - Expected answer: Implement `MovieRepository` with methods for searching by title, genre, and rating.

4. **Create a REST API endpoint to get a list of all movies.**
   - Expected answer: Implement `getAllMovies()` method in `MovieController` with `@GetMapping`.

5. **Write a REST API endpoint to search for movies by title.**
   - Expected answer: Add a method in `MovieController` with `@GetMapping("/search")` and search logic by title.

6. **Write a method to save a new movie in the database via POST request.**
   - Expected answer: Implement `createMovie()` method in `MovieController` with `@PostMapping`.

7. **Write a service method to update an existing movie’s details.**
   - Expected answer: Implement `updateMovie()` in `MovieService` and call it from `MovieController`.

8. **Write an API endpoint to delete a movie by its ID.**
   - Expected answer: Implement `deleteMovie()` in `MovieController` with `@DeleteMapping`.

9. **Implement a method to find all movies with a rating greater than or equal to a given value.**
   - Expected answer: Write a query in `MovieRepository` with `@Query`.

10. **Write a method in `MovieRepository` to fetch movies released between two dates.**
    - Expected answer: Define a custom query using `@Query` and `@Param` annotations.

11. **Create an exception handler for when a movie is not found.**
    - Expected answer: Implement `ResourceNotFoundException` and throw it from service methods.

12. **Write the configuration to connect to an in-memory H2 database.**
    - Expected answer: Define properties in `application.properties` for H2 configuration.

13. **Write code to enable CORS for all origins and HTTP methods in your API.**
    - Expected answer: Implement `WebConfig` with `addCorsMappings()` method.

14. **Add pagination support to the API to get movies page by page.**
    - Expected answer: Modify `MovieRepository` to extend `PagingAndSortingRepository`.

15. **Write a query to find movies by director name.**
    - Expected answer: Add a query in `MovieRepository` to search by `director`.

16. **Implement a method to search for movies by genre.**
    - Expected answer: Write `findByGenre()` in `MovieRepository` and use it in `MovieService`.

17. **Create a method in the service layer to fetch a movie by its ID.**
    - Expected answer: Implement `getMovieById()` in `MovieService`.

18. **Write a method to update only the rating of a movie.**
    - Expected answer: Implement a partial update method in `MovieService`.

19. **Add validation so that the rating must be between 0 and 10.**
    - Expected answer: Use `@Min` and `@Max` annotations in the `Movie` entity for the rating field.

20. **Modify the `Movie` entity to make the `releaseDate` mandatory.**
    - Expected answer: Add `@NotNull` annotation to the `releaseDate` field.

21. **Write code to automatically generate the `id` for each movie.**
    - Expected answer: Use `@GeneratedValue(strategy = GenerationType.IDENTITY)` in `Movie`.

22. **Implement a custom exception handler for invalid input.**
    - Expected answer: Create a class annotated with `@ControllerAdvice` to handle validation errors.

23. **Write a method to search for movies by title, ignoring case.**
    - Expected answer: Implement `findByTitleContainingIgnoreCase()` in `MovieRepository`.

24. **Add functionality to return only movies of a specific genre via query parameters.**
    - Expected answer: Add query parameter handling in `MovieController`.

25. **Write a method to filter movies based on rating via query parameters.**
    - Expected answer: Modify `MovieController` to filter by rating.

26. **Write code to log every request made to the `/api/movies` endpoint.**
    - Expected answer: Use `@Before` in an Aspect or Spring Boot's logging functionality.

27. **Implement a feature to soft delete movies (i.e., mark them as deleted without removing them from the database).**
    - Expected answer: Add a `deleted` flag in the `Movie` entity and update `deleteMovie()` method.

28. **Write a method to retrieve only non-deleted movies from the database.**
    - Expected answer: Modify queries in `MovieRepository` to filter out deleted movies.

29. **Write the properties to show SQL queries in the console.**
    - Expected answer: Add `spring.jpa.show-sql=true` in `application.properties`.

30. **Write a unit test for the `createMovie()` method.**
    - Expected answer: Use JUnit and Mockito to write a test case for `createMovie()`.

31. **Write a unit test for the `getAllMovies()` method.**
    - Expected answer: Mock the repository and test the `getAllMovies()` method.

32. **Write a REST API method to return the number of movies in the database.**
    - Expected answer: Add an endpoint in `MovieController` to return the count of movies.

33. **Implement internationalization for movie titles in the API.**
    - Expected answer: Use Spring's message sources to return titles in different languages.

34. **Write a method to update a movie’s genre without modifying other fields.**
    - Expected answer: Create a partial update in `MovieService`.

35. **Write code to handle invalid data in the `rating` field and return an appropriate error message.**
    - Expected answer: Add validation and exception handling for `@Min` and `@Max`.

36. **Write code to fetch a list of movies sorted by their release date.**
    - Expected answer: Use `findAll(Sort.by("releaseDate"))` in `MovieRepository`.

37. **Write a custom validation annotation to check if the movie title is unique.**
    - Expected answer: Implement a custom `@UniqueTitle` validation annotation.

38. **Write a method to find all movies directed by a specific director.**
    - Expected answer: Add a method in `MovieRepository` and call it from `MovieService`.

39. **Implement basic authentication for accessing the movie API.**
    - Expected answer: Use Spring Security for implementing basic authentication.

40. **Write a method to return a list of top-rated movies (rating > 8).**
    - Expected answer: Implement a query in `MovieRepository` to fetch top-rated movies.

41. **Write a search endpoint to filter movies by both title and genre.**
    - Expected answer: Modify `MovieController` to accept both title and genre as query parameters.

42. **Add Swagger documentation to the movie API.**
    - Expected answer: Integrate Swagger for API documentation.

43. **Write a method to find movies where the title starts with a given string.**
    - Expected answer: Use a custom query in `MovieRepository`.

44. **Write code to validate the release date so that it cannot be in the future.**
    - Expected answer: Add a custom validator for the `releaseDate` field.

45. **Create a custom exception for duplicate movie titles.**
    - Expected answer: Throw a custom `DuplicateTitleException` in `createMovie()`.

46. **Write a method to retrieve movies released in a specific year.**
    - Expected answer: Use `findByReleaseDateBetween()` to filter movies by release year.

47. **Implement a method to handle HTTP `400` errors globally.**
    - Expected answer: Create a global exception handler with `@ControllerAdvice`.

48. **Write a test to verify that the movie repository saves a movie correctly.**
    - Expected answer: Write an integration test using H2 to test `save()` method.

49. **Write code to validate that the movie description is not too short.**
    - Expected answer: Use `@Size(min = 10)` annotation in the `Movie` entity.

50. **Add logging in the service layer to log when movies are created, updated, or deleted.**
    - Expected answer: Use SLF4J or Logback for logging in `MovieService`.

These questions aim to assess students' coding skills and their ability to implement and work with various Spring Boot concepts.

# 100 Questions and Answers to Test Understanding of the Movie API Project

Below are 100 questions and answers designed to test and reinforce your understanding of the Movie API project we discussed earlier. These questions cover various aspects of the project, including annotations, architecture, code implementation, and best practices.

---

## General Project Understanding

**1. What is the primary purpose of the Movie API project?**

**Answer:**
The primary purpose of the Movie API project is to create a RESTful web service similar to the TMDB API, allowing clients to perform CRUD operations on movie data. It provides endpoints to create, read, update, delete, and search movies.

---

**2. Which frameworks and technologies are used in this project?**

**Answer:**
The project uses Spring Boot, Spring Data JPA, Hibernate, H2 Database (in-memory database for development), and optionally Lombok for reducing boilerplate code.

---

**3. Why is Spring Boot chosen for this project?**

**Answer:**
Spring Boot is chosen because it simplifies application setup by providing auto-configuration and starter dependencies, reduces boilerplate code, and accelerates development. It also integrates seamlessly with Spring Data JPA and other Spring projects.

---

## Application Entry Point

**4. What is the role of the `MovieApiApplication` class?**

**Answer:**
The `MovieApiApplication` class is the entry point of the Spring Boot application. It contains the `main` method that bootstraps the application using `SpringApplication.run()`.

---

**5. What does the `@SpringBootApplication` annotation do?**

**Answer:**
The `@SpringBootApplication` annotation enables auto-configuration, component scanning, and allows Spring Boot to start adding beans based on classpath settings, other beans, and various property settings.

---

## Entity Layer

**6. What is the purpose of the `@Entity` annotation on the `Movie` class?**

**Answer:**
The `@Entity` annotation specifies that the `Movie` class is a JPA entity, meaning it maps to a database table and will be managed by the JPA provider (Hibernate).

---

**7. How does the `@Table(name = "movies")` annotation affect the `Movie` entity?**

**Answer:**
The `@Table(name = "movies")` annotation specifies that the `Movie` entity should be mapped to the database table named "movies" instead of the default table name derived from the class name.

---

**8. What is the purpose of the `@Id` and `@GeneratedValue` annotations on the `id` field?**

**Answer:**
The `@Id` annotation marks the `id` field as the primary key of the entity. The `@GeneratedValue(strategy = GenerationType.IDENTITY)` annotation indicates that the primary key value will be automatically generated by the database using an identity column.

---

**9. How are validation constraints applied to the `Movie` entity fields?**

**Answer:**
Validation constraints are applied using annotations from the `javax.validation.constraints` package, such as `@NotBlank`, `@Min`, and `@Max`, to enforce rules like non-empty titles and rating ranges.

---

**10. What data types are used for the `rating` and `releaseDate` fields, and why?**

**Answer:**
The `rating` field uses `Double` to allow decimal values for ratings, accommodating fractional ratings (e.g., 7.5). The `releaseDate` field uses `LocalDate` from Java 8 Date and Time API to represent dates without time zones.

---

## Repository Layer

**11. Why does `MovieRepository` extend `JpaRepository<Movie, Long>`?**

**Answer:**
By extending `JpaRepository<Movie, Long>`, `MovieRepository` inherits CRUD operations and JPA-related methods for the `Movie` entity with a primary key of type `Long`.

---

**12. What is the significance of the `@Repository` annotation on `MovieRepository`?**

**Answer:**
The `@Repository` annotation indicates that `MovieRepository` is a Spring Data Repository, which may enable exception translation. However, it's optional because Spring Data detects repository interfaces automatically.

---

**13. How does the method `List<Movie> findByTitleContainingIgnoreCase(String title)` work?**

**Answer:**
This method allows searching for movies whose titles contain a specific string, ignoring case sensitivity. Spring Data JPA parses the method name and creates a query accordingly.

---

**14. What does the `@Query` annotation do in `findByRatingGreaterThanEqual` method?**

**Answer:**
The `@Query` annotation allows defining a custom JPQL query. In this case, it retrieves movies with a rating greater than or equal to the specified value.

---

**15. Explain the use of `@Param("rating")` in the `findByRatingGreaterThanEqual` method.**

**Answer:**
`@Param("rating")` binds the method parameter `rating` to the named parameter `:rating` in the JPQL query, ensuring the value is correctly passed to the query.

---

## Service Layer

**16. What is the role of the `MovieService` class?**

**Answer:**
`MovieService` encapsulates business logic and acts as an intermediary between the controller and repository layers, handling data processing and manipulation before passing data to or from the repository.

---

**17. Why is constructor-based dependency injection used in `MovieService`?**

**Answer:**
Constructor-based dependency injection is preferred because it allows for immutability, easier testing (through mocking), and ensures that required dependencies are not `null`.

---

**18. How does the `updateMovie` method ensure transactional integrity?**

**Answer:**
The `updateMovie` method is annotated with `@Transactional`, which ensures that all operations within the method are executed within a transaction. If any operation fails, the transaction is rolled back.

---

**19. What exception is thrown if a movie to be updated or deleted does not exist?**

**Answer:**
A custom `ResourceNotFoundException` is thrown, indicating that the requested movie could not be found.

---

**20. Why are there separate methods in `MovieService` for different search criteria?**

**Answer:**
Having separate methods for different search criteria improves code organization, readability, and allows for reusability in different parts of the application.

---

## Controller Layer

**21. What is the purpose of the `@RestController` annotation on `MovieController`?**

**Answer:**
`@RestController` marks the class as a RESTful controller, meaning it handles HTTP requests and automatically serializes return values into JSON or XML responses.

---

**22. What does the `@RequestMapping("/api/movies")` annotation do?**

**Answer:**
It sets the base URL path for all endpoints in `MovieController` to `/api/movies`, so all methods within will be accessible under this path.

---

**23. How does the `getMovieById` method handle the scenario where the movie is not found?**

**Answer:**
It uses `orElseThrow()` on the `Optional<Movie>` returned by `movieService.getMovieById(id)`, throwing a `ResourceNotFoundException` if the movie is not present.

---

**24. Explain the use of `@Valid` in controller methods that accept `@RequestBody` parameters.**

**Answer:**
`@Valid` triggers the validation constraints defined on the `Movie` entity fields when a new movie is created or updated, ensuring that the data meets the specified requirements.

---

**25. How does the `searchMovies` method in `MovieController` determine which search criteria to use?**

**Answer:**
It checks which query parameters are not `null` and calls the corresponding service method based on which parameters are provided, allowing for flexible search capabilities.

---

**26. Why is `ResponseEntity` used in some controller methods?**

**Answer:**
`ResponseEntity` allows for more control over the HTTP response, including setting the status code, headers, and body. It provides flexibility in crafting the response returned to the client.

---

**27. What HTTP methods are mapped to the CRUD operations in the controller?**

**Answer:**
- **GET**: Retrieve data (`getAllMovies`, `getMovieById`, `searchMovies`)
- **POST**: Create new data (`createMovie`)
- **PUT**: Update existing data (`updateMovie`)
- **DELETE**: Remove data (`deleteMovie`)

---

**28. How are path variables and request parameters used in the controller methods?**

**Answer:**
- **Path Variables**: Defined using `@PathVariable`, they capture values from the URI path (e.g., `/api/movies/{id}`).
- **Request Parameters**: Defined using `@RequestParam`, they capture query parameters from the URL (e.g., `/api/movies/search?title=Inception`).

---

**29. Why is `HttpStatus.CREATED` used in the `createMovie` method?**

**Answer:**
`HttpStatus.CREATED` (HTTP status code 201) indicates that a new resource has been successfully created. It's appropriate to use when responding to POST requests that create new data.

---

**30. What is the purpose of `@DateTimeFormat(iso = DateTimeFormat.ISO.DATE)` in the `searchMoviesByReleaseDateRange` method parameters?**

**Answer:**
It specifies the expected date format for parsing date strings from the request parameters, ensuring that `LocalDate` objects are correctly instantiated from the input.

---

## Exception Handling

**31. What does the `@ResponseStatus(HttpStatus.NOT_FOUND)` annotation do in `ResourceNotFoundException`?**

**Answer:**
It marks the exception to return an HTTP 404 Not Found status code when the exception is thrown, informing the client that the requested resource could not be found.

---

**32. How does the application handle validation errors when invalid data is provided?**

**Answer:**
When invalid data is submitted (violating validation constraints), Spring's `MethodArgumentNotValidException` is thrown, which by default results in a 400 Bad Request response with error details.

---

**33. How can you customize the error response for exceptions like `ResourceNotFoundException`?**

**Answer:**
By creating a `@ControllerAdvice` class with `@ExceptionHandler` methods, you can define custom error responses and messages for specific exceptions.

---

**34. Why is it important to handle exceptions properly in a RESTful API?**

**Answer:**
Proper exception handling ensures that clients receive meaningful and standardized error responses, improving the API's usability and debuggability.

---

## Validation

**35. What is the purpose of using validation annotations like `@NotBlank` and `@Max`?**

**Answer:**
Validation annotations enforce constraints on entity fields to ensure data integrity and prevent invalid data from being persisted to the database.

---

**36. How does `@Valid` interact with the validation annotations in the `Movie` entity?**

**Answer:**
When `@Valid` is used on a method parameter, Spring automatically validates the object against its validation annotations before the method executes, throwing an exception if validation fails.

---

**37. What happens if you try to save a `Movie` entity with a null `title`?**

**Answer:**
A validation error occurs due to the `@NotBlank` annotation on the `title` field, and a `MethodArgumentNotValidException` is thrown, resulting in a 400 Bad Request response.

---

**38. How can you provide custom validation messages for the constraints?**

**Answer:**
By setting the `message` attribute on the validation annotations, e.g., `@NotBlank(message = "Title is mandatory")`.

---

**39. How would you add a custom validation constraint to the `genre` field to allow only specific genres?**

**Answer:**
You can create a custom validation annotation or use `@Pattern` to define a regular expression that matches allowed genres. Alternatively, you can use `@Enumerated` with an `enum` of allowed genres.

---

## Configuration and Properties

**40. What is the purpose of the `application.properties` file?**

**Answer:**
`application.properties` contains configuration settings for the Spring Boot application, such as database connection details, Hibernate settings, and other application-level properties.

---

**41. How does the application connect to the H2 in-memory database?**

**Answer:**
By specifying the JDBC URL, driver class name, username, and password in `application.properties`. Spring Boot auto-configures the data source based on these properties.

---

**42. What does `spring.jpa.hibernate.ddl-auto=update` do?**

**Answer:**
It tells Hibernate to automatically update the database schema to match the entities each time the application runs, adding new tables or columns as needed without dropping existing data.

---

**43. Why might you set `spring.jpa.show-sql=true` in `application.properties`?**

**Answer:**
To enable logging of SQL statements executed by Hibernate, which is helpful for debugging and understanding the generated queries.

---

**44. How can you change the logging level for SQL statements to see parameter values?**

**Answer:**
By setting `logging.level.org.hibernate.type.descriptor.sql=TRACE` in `application.properties`, you can see the parameter bindings in the logs.

---

## Dependency Injection and Bean Management

**45. What is dependency injection, and how is it implemented in this project?**

**Answer:**
Dependency injection is a design pattern where dependencies are provided to a class rather than the class instantiating them itself. In this project, it's implemented using constructor-based injection, where dependencies are passed via the constructor.

---

**46. Why is constructor-based injection preferred over field injection?**

**Answer:**
Constructor-based injection is preferred because it allows for immutability, easier testing, and ensures that the dependency is not `null` since it's required at object creation.

---

**47. How does Spring know which beans to inject into the `MovieService` and `MovieController`?**

**Answer:**
Spring scans for components (classes annotated with `@Component`, `@Service`, `@Repository`, `@Controller`, etc.) and manages them as beans in the application context. It matches the required types in constructors to available beans.

---

**48. What would happen if there are multiple beans of the same type? How can you resolve this?**

**Answer:**
Spring would throw a `NoUniqueBeanDefinitionException` due to ambiguity. You can resolve this by using `@Qualifier` annotations to specify which bean to inject or by defining one as the primary bean using `@Primary`.

---

## Transaction Management

**49. How does the `@Transactional` annotation affect the `updateMovie` method?**

**Answer:**
It ensures that all database operations within the `updateMovie` method are executed within a single transaction. If any operation fails, the entire transaction is rolled back to maintain data integrity.

---

**50. Can `@Transactional` be applied at the class level? What effect does this have?**

**Answer:**
Yes, `@Transactional` can be applied at the class level, making all public methods of the class transactional by default.

---

**51. What isolation level and propagation settings are used by default with `@Transactional`?**

**Answer:**
By default, the isolation level is `DEFAULT` (uses the database's default), and the propagation is `REQUIRED`, meaning it will join an existing transaction or create a new one if none exists.

---

**52. How can you customize the transaction settings if needed?**

**Answer:**
By specifying attributes in the `@Transactional` annotation, such as `isolation`, `propagation`, `readOnly`, `timeout`, and `rollbackFor`.

---

## CORS Configuration

**53. What is CORS, and why is it important in web applications?**

**Answer:**
CORS (Cross-Origin Resource Sharing) is a security feature that allows or restricts resources to be requested from another domain outside the domain from which the resource originated. It's important for controlling access to your API from different origins.

---

**54. How is CORS configured in the `WebConfig` class?**

**Answer:**
By implementing `WebMvcConfigurer` and overriding the `addCorsMappings` method, where specific paths, allowed origins, and allowed methods are defined.

---

**55. What does `allowedOrigins("*")` signify in the CORS configuration?**

**Answer:**
It allows requests from any origin. In production, it's recommended to specify exact origins to enhance security.

---

**56. How can you restrict CORS to allow only specific HTTP methods?**

**Answer:**
By specifying the methods in `allowedMethods()`, e.g., `.allowedMethods("GET", "POST")`, you can control which HTTP methods are permitted.

---

## Project Structure and Organization

**57. Why is it important to have a clear package structure like `controller`, `service`, `repository`, `entity`, and `config`?**

**Answer:**
A clear package structure enhances code organization, maintainability, and readability by logically separating different layers and concerns of the application.

---

**58. How does component scanning work in this project?**

**Answer:**
Spring Boot automatically scans the package where the main application class resides and its sub-packages for components annotated with stereotypes (`@Component`, `@Service`, etc.), registering them as beans.

---

**59. What would happen if you placed a component outside the scanned packages?**

**Answer:**
Spring would not detect or register it as a bean, leading to `NoSuchBeanDefinitionException` when trying to inject it elsewhere.

---

## Testing the API

**60. What tools can you use to test the Movie API endpoints?**

**Answer:**
Tools like Postman, cURL, or any HTTP client can be used to send requests to the API endpoints and observe responses.

---

**61. How can you test that the `createMovie` endpoint correctly validates input data?**

**Answer:**
By sending POST requests with both valid and invalid payloads to the `/api/movies` endpoint and verifying that valid data is accepted and invalid data results in appropriate error responses.

---

**62. How would you verify that the search functionality works as expected?**

**Answer:**
By making GET requests to `/api/movies/search` with different query parameters and confirming that the returned results match the search criteria.

---

## Advanced Topics and Best Practices

**63. How can you implement pagination in the `getAllMovies` method?**

**Answer:**
By modifying the method to accept `Pageable` as a parameter and returning a `Page<Movie>` from the repository, enabling pagination and sorting features.

---

**64. Why is it important to handle exceptions like `ResourceNotFoundException` at the controller level?**

**Answer:**
To provide meaningful and user-friendly error messages to the client and to ensure that the correct HTTP status codes are returned.

---

**65. How can you secure the Movie API endpoints?**

**Answer:**
By integrating Spring Security to handle authentication and authorization, defining security configurations, and protecting endpoints based on user roles or permissions.

---

**66. What is the purpose of the `@Configuration` annotation on the `WebConfig` class?**

**Answer:**
It marks the class as a source of bean definitions for the application context, indicating that it contains configuration settings (e.g., CORS mappings).

---

**67. How can you document the API endpoints for consumers?**

**Answer:**
By using tools like Swagger (OpenAPI) to generate interactive API documentation or Spring REST Docs to create documentation based on tests.

---

**68. What are the benefits of using an in-memory database like H2 during development?**

**Answer:**
It simplifies setup by not requiring an external database, speeds up development, and allows for easy testing without affecting production data.

---

**69. How can you switch from H2 to a production database like PostgreSQL?**

**Answer:**
By updating the `application.properties` with the PostgreSQL JDBC URL, driver class name, username, and password, and including the PostgreSQL dependency in the project's build configuration.

---

**70. What is Lombok, and how can it be used in this project?**

**Answer:**
Lombok is a Java library that reduces boilerplate code using annotations. For example, `@Data` generates getters, setters, and other utility methods automatically.

---

## Code Maintenance and Scalability

**71. How does separating concerns into different layers (controller, service, repository) help in code maintenance?**

**Answer:**
It promotes modularity, making it easier to manage, test, and modify each layer independently without affecting others, enhancing scalability and maintainability.

---

**72. How would you handle additional features like user reviews or ratings in the API?**

**Answer:**
By creating new entities (e.g., `Review`), repositories, services, and controllers to manage the new data, ensuring adherence to the established architectural patterns.

---

**73. How can you ensure that your API scales with increased traffic and data?**

**Answer:**
By optimizing queries, using pagination, implementing caching mechanisms, load balancing, and potentially migrating to more scalable database solutions.

---

**74. Why is it important to write unit tests and integration tests for the API?**

**Answer:**
To verify that individual components work correctly (unit tests) and that the system as a whole functions as expected (integration tests), reducing bugs and ensuring reliability.

---

**75. How can you mock dependencies when writing unit tests for the service layer?**

**Answer:**
By using mocking frameworks like Mockito to create mock instances of dependencies (e.g., `MovieRepository`) and defining their behavior during tests.

---

## Performance Optimization

**76. How can you improve the performance of database queries in the repository?**

**Answer:**
By optimizing queries, adding indexes to frequently queried columns, using pagination, and avoiding N+1 select issues with fetch strategies.

---

**77. What is the N+1 select problem, and how can it be mitigated in JPA?**

**Answer:**
The N+1 select problem occurs when one query triggers additional queries for each result (e.g., fetching related entities lazily). It can be mitigated by using `@EntityGraph` or `JOIN FETCH` in queries to fetch related data eagerly.

---

**78. How can caching be implemented to reduce database load?**

**Answer:**
By using caching mechanisms like Spring Cache, Redis, or Hibernate's second-level cache to store frequently accessed data in memory.

---

## Security Considerations

**79. Why should `allowedOrigins("*")` be avoided in production CORS configurations?**

**Answer:**
Allowing all origins can expose the API to Cross-Site Request Forgery (CSRF) attacks and other security risks. It's better to specify trusted domains.

---

**80. How can input validation help in preventing security vulnerabilities?**

**Answer:**
Input validation ensures that only valid and expected data is processed, preventing injection attacks like SQL injection or cross-site scripting (XSS).

---

**81. What headers can be added to enhance the security of API responses?**

**Answer:**
Headers like `Content-Security-Policy`, `X-Content-Type-Options`, `Strict-Transport-Security`, and `X-Frame-Options` can help mitigate various attacks.

---

## API Design Principles

**82. Why is it important to use proper HTTP status codes in API responses?**

**Answer:**
Proper HTTP status codes communicate the result of the request to the client accurately, aiding in error handling and debugging.

---

**83. How does using RESTful principles improve the API design?**

**Answer:**
RESTful principles promote statelessness, resource-based URLs, and the use of standard HTTP methods, making the API intuitive and interoperable.

---

**84. What is HATEOAS, and how can it be applied to this API?**

**Answer:**
HATEOAS (Hypermedia as the Engine of Application State) is a principle where API responses include links to related resources. It can be applied by adding hypermedia links in responses for navigation.

---

## Advanced JPA Features

**85. How can you perform bulk updates or deletes in Spring Data JPA?**

**Answer:**
By using the `@Modifying` annotation on repository methods that execute update or delete queries.

---

**86. What is the purpose of the `@EntityListeners` annotation in JPA entities?**

**Answer:**
It allows entities to respond to lifecycle events (e.g., pre-persist, post-load) by specifying callback methods or listeners.

---

**87. How can you map complex relationships between entities, such as one-to-many or many-to-many?**

**Answer:**
By using JPA annotations like `@OneToMany`, `@ManyToOne`, `@ManyToMany`, and configuring the mapping details (e.g., `mappedBy`, `joinTable`).

---

## Logging and Monitoring

**88. How can you implement logging in the application for debugging purposes?**

**Answer:**
By using logging frameworks like SLF4J with Logback or Log4j, and adding log statements at appropriate levels (e.g., debug, info, error).

---

**89. What is Spring Boot Actuator, and how can it help in monitoring the application?**

**Answer:**
Spring Boot Actuator provides production-ready features like health checks, metrics, and monitoring endpoints, helping to manage and monitor the application.

---

**90. How can you use metrics collected by Actuator to improve the application?**

**Answer:**
By analyzing metrics on request counts, response times, error rates, you can identify bottlenecks, optimize performance, and improve reliability.

---

## Continuous Integration and Deployment

**91. How would you set up continuous integration for this project?**

**Answer:**
By using CI tools like Jenkins, Travis CI, or GitHub Actions to automate building, testing, and validating code changes before merging.

---

**92. What steps are involved in deploying this application to a production environment?**

**Answer:**
- Build the application (e.g., generate a JAR file).
- Provision a server or use a cloud platform.
- Configure environment variables and production database.
- Deploy the application to the server.
- Set up monitoring and logging.
- Secure the application endpoints.

---

## Miscellaneous

**93. How can you externalize configuration properties for different environments (dev, test, prod)?**

**Answer:**
By using Spring Profiles and creating separate `application-{profile}.properties` files for each environment, activating the desired profile at runtime.

---

**94. What is the purpose of the `@Data` annotation from Lombok?**

**Answer:**
`@Data` generates getters, setters, `toString()`, `equals()`, and `hashCode()` methods, reducing boilerplate code in entities and data classes.

---

**95. How does the application handle serialization and deserialization of JSON data?**

**Answer:**
Spring Boot uses Jackson by default to automatically serialize Java objects to JSON and deserialize JSON to Java objects when handling HTTP requests and responses.

---

**96. How can you customize the JSON serialization process, for example, to exclude certain fields?**

**Answer:**
By using annotations like `@JsonIgnore` on fields to exclude them or configuring custom serializers and deserializers.

---

**97. What is the significance of the `serialVersionUID` in entities?**

**Answer:**
While not shown in the project, `serialVersionUID` is used in Java serialization to ensure that a deserialized object matches the version of the class.

---

**98. How can you handle versioning of your API endpoints?**

**Answer:**
By including version numbers in the URL paths (e.g., `/api/v1/movies`) or using headers to indicate the API version, allowing for changes without breaking existing clients.

---

**99. What are the benefits of using DTOs (Data Transfer Objects) instead of entities in controller methods?**

**Answer:**
Using DTOs decouples the API layer from the persistence layer, allows for customized response formats, enhances security by exposing only necessary data, and prevents unintentional modifications to entities.

---

**100. How can you improve the security of your application against common web vulnerabilities?**

**Answer:**
- Implement input validation and sanitization.
- Use prepared statements to prevent SQL injection.
- Secure endpoints with authentication and authorization.
- Use HTTPS to encrypt data in transit.
- Keep dependencies up to date to patch known vulnerabilities.

---

# Conclusion

These 100 questions and answers cover a comprehensive range of topics related to the Movie API project. They are designed to test your understanding of the implementation details, best practices, and architectural decisions made in building a Spring Boot RESTful API using Spring Data JPA. Reviewing and answering these questions should reinforce your knowledge and help identify areas where further study might be beneficial.

---------------------------------------
