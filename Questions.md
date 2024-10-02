### 1. **What are the three main annotations combined in `@SpringBootApplication`?**

The `@SpringBootApplication` annotation is like a shortcut in Spring Boot that combines three important annotations:

1. **`@Configuration`**: Marks the class as a source of bean definitions for the application context.
2. **`@EnableAutoConfiguration`**: Tells Spring Boot to automatically configure your application based on the dependencies you've added.
3. **`@ComponentScan`**: Instructs Spring to scan for components, configurations, and services in the specified package.

By using `@SpringBootApplication`, you don't have to annotate your main class with these three annotations separately.

---

### 2. **What is the purpose of the `@Configuration` annotation?**

The `@Configuration` annotation indicates that a class declares one or more `@Bean` methods. These methods create beans that are managed by the Spring container. Essentially, `@Configuration` classes are used to define beans and dependency injection in a declarative way.

---

### 3. **How does `@EnableAutoConfiguration` contribute to a Spring Boot application?**

`@EnableAutoConfiguration` tells Spring Boot to automatically configure your application based on the dependencies present on the classpath. For example, if you have the `spring-web` dependency, it will set up a web server automatically. This saves you from manually configuring many settings and lets you focus on writing your application logic.

---

### 4. **What does the `@ComponentScan` annotation do in a Spring Boot application?**

`@ComponentScan` tells Spring where to look for components like controllers, services, and repositories. It scans the specified package and its sub-packages to find classes annotated with `@Component`, `@Service`, `@Repository`, `@Controller`, etc., and registers them as beans in the application context.

---

### 5. **In the `DemoApplication` class, what method is used to start the Spring Boot application?**

In the `DemoApplication` class, the `main` method is used to start the application. It calls `SpringApplication.run(DemoApplication.class, args);`, which bootstraps the application, starting the embedded server and initializing the Spring context.

---

### 6. **What is Dependency Injection, and how is it implemented in the Controller class?**

**Dependency Injection (DI)** is a design pattern where an object's dependencies are provided by an external entity rather than the object creating them itself. This promotes loose coupling and easier testing.

In the Controller class, DI is implemented through constructor-based injection:

```java
@RestController
public class Controller {
    private final MovieServices ms;

    public Controller(MovieServices ms) {
        this.ms = ms;
    }
    // ...
}
```

Here, `MovieServices` is injected into the `Controller` class via the constructor, and Spring automatically provides an instance of `MovieServices` when creating the `Controller` bean.

---

### 7. **What are the benefits of using constructor-based dependency injection?**

Benefits include:

- **Immutability**: Dependencies are set once at construction, promoting immutable objects.
- **Testability**: Easier to write unit tests by injecting mock dependencies.
- **Mandatory Dependencies**: Ensures that required dependencies are provided.
- **Thread Safety**: Immutable dependencies can improve thread safety.
- **Clear Contracts**: The constructor clearly defines what the class needs to function.

---

### 8. **What is the purpose of the `@RestController` annotation?**

`@RestController` is a convenience annotation that combines `@Controller` and `@ResponseBody`. It indicates that the class handles HTTP requests and returns data directly in the response body, usually in JSON format. This is typically used for building RESTful web services.

---

### 9. **How does `@RestController` differ from `@Controller`?**

- **`@Controller`**: Used in MVC applications where methods return view names, and data is rendered in templates.
- **`@RestController`**: Methods return data directly (e.g., JSON), and `@ResponseBody` is implicitly added to all methods.

So, `@RestController` is specialized for RESTful services, eliminating the need to annotate each method with `@ResponseBody`.

---

### 10. **What is the role of the `@Service` annotation in Spring Boot applications?**

The `@Service` annotation marks a class as a service provider. It's a specialization of `@Component`, indicating that it holds business logic. This helps in component scanning and makes the application's structure clearer by separating service-layer components.

---

### 11. **How does the `@Service` annotation relate to the `@Component` annotation?**

`@Service` is a stereotype annotation derived from `@Component`. While `@Component` is a generic annotation for any Spring-managed component, `@Service` is more specific and indicates that the class provides business functionalities. It's mainly for developer readability and organization.

---

### 12. **What is the primary function of the `@Repository` annotation?**

The `@Repository` annotation indicates that the class or interface is a data access component. It:

- Enables automatic exception translation from database-specific exceptions to Spring's `DataAccessException`.
- Marks the class for component scanning, so Spring can detect and register it.

---

### 13. **How does the `@Repository` annotation handle database exceptions?**

`@Repository` provides a translation layer that converts database-specific exceptions (like `SQLException`) into Spring's `DataAccessException` hierarchy. This unifies exception handling across different databases and simplifies error management.

---

### 14. **What does the `@Entity` annotation signify in JPA?**

`@Entity` marks a class as a JPA entity, meaning it's mapped to a database table. This tells the JPA provider (like Hibernate) that instances of this class should be persisted to the database.

---

### 15. **How is the `@Table` annotation used in conjunction with `@Entity`?**

`@Table` specifies the table name in the database that the entity maps to. If the class name and table name differ, `@Table(name = "table_name")` is used to define the mapping explicitly.

---

### 16. **What is the purpose of extending `JpaRepository` in Spring Data JPA?**

Extending `JpaRepository` provides CRUD operations and pagination out of the box. It saves you from writing boilerplate code for common database operations and allows you to define custom query methods easily.

---

### 17. **How does Spring Data JPA simplify data access layers?**

Spring Data JPA:

- Provides default implementations for common data access methods.
- Supports custom query methods based on method naming conventions.
- Allows custom queries with `@Query`.
- Handles transactions automatically.

This reduces the amount of code you need to write for data access.

---

### 18. **What is the purpose of the `@Query` annotation in Spring Data JPA?**

The `@Query` annotation allows you to define custom queries using JPQL or native SQL directly in repository methods. It's used when method naming conventions aren't sufficient for complex queries.

---

### 19. **How can you write custom queries using the `@Query` annotation?**

You can write a JPQL or SQL query as a string inside the `@Query` annotation and use method parameters with `@Param` to bind values.

Example:

```java
@Query("SELECT m FROM MovieApp m WHERE m.genre = :genre")
List<MovieApp> findByGenre(@Param("genre") String genre);
```

---

### 20. **What is the difference between JPQL and native SQL in `@Query` annotations?**

- **JPQL (Java Persistence Query Language)**: Object-oriented, uses entity and field names, database-independent.
- **Native SQL**: Uses actual SQL syntax, interacts directly with the database tables and columns.

In `@Query`, you can specify `nativeQuery = true` to use native SQL.

---

### 21. **What does the `@Transactional` annotation do in Spring Boot applications?**

`@Transactional` manages transactions declaratively. It ensures that a method executes within a transaction context, handling commit or rollback automatically based on method execution and exceptions.

---

### 22. **At what levels can the `@Transactional` annotation be applied?**

- **Method Level**: Applies to individual methods.
- **Class Level**: Applies to all public methods in the class.

Method-level annotations override class-level settings.

---

### 23. **How does Spring Boot handle transaction management when using `@Transactional`?**

Spring Boot uses AOP (Aspect-Oriented Programming) proxies to intercept methods annotated with `@Transactional`. Before the method executes, a transaction is started. After the method completes, the transaction is committed or rolled back based on whether an exception occurred.

---

### 24. **What is the purpose of the `@Configuration` annotation in the `WebConfig` class?**

In `WebConfig`, `@Configuration` marks the class as a source of bean definitions. It allows the class to be processed by the Spring container, enabling customization of Spring MVC configurations, such as CORS settings.

---

### 25. **How is CORS (Cross-Origin Resource Sharing) configured in the `WebConfig` class?**

CORS is configured by implementing `WebMvcConfigurer` and overriding `addCorsMappings`. In `WebConfig`, you specify allowed origins, methods, headers, and whether credentials are allowed.

Example:

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(@NonNull CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedOrigins("http://localhost:3000")
                .allowedMethods("GET", "POST", "PUT", "DELETE", "OPTIONS")
                .allowedHeaders("*")
                .allowCredentials(true);
    }
}
```

---

### 26. **What is the relationship between Spring Boot and Spring Data JPA?**

Spring Boot and Spring Data JPA work together to simplify data access:

- **Spring Boot**: Auto-configures JPA settings, entity scanning, and transaction management.
- **Spring Data JPA**: Provides repository abstractions to simplify database interactions.

Spring Boot sets up everything needed to use Spring Data JPA with minimal configuration.

---

### 27. **How does Spring Data JPA relate to the Jakarta Persistence API?**

Spring Data JPA builds on the Jakarta Persistence API (JPA):

- **JPA**: Defines the standard for ORM in Java, including annotations and interfaces for entity management.
- **Spring Data JPA**: Extends JPA, providing repository interfaces and additional features to simplify data access.

---

### 28. **What role does Hibernate play in a Spring Boot application using JPA?**

Hibernate is the default JPA provider in Spring Boot. It implements the JPA specifications, handling the ORM tasks like translating Java entities to database tables, managing sessions, and executing queries.

---

### 29. **In the technology stack, where does the database fit in relation to Hibernate?**

The database is at the bottom of the stack:

1. **Application Code**: Uses JPA annotations and interfaces.
2. **Hibernate (JPA Provider)**: Implements JPA, translating application operations to database commands.
3. **Database**: Stores the actual data.

Hibernate acts as the bridge between your application and the database.

---

### 30. **How does Spring Boot simplify the configuration of JPA and Hibernate?**

Spring Boot auto-configures JPA and Hibernate based on the dependencies and properties:

- Detects `spring-boot-starter-data-jpa` and sets up JPA configurations.
- Scans for entities automatically.
- Configures `EntityManagerFactory` and transaction management.
- Uses sensible defaults, reducing the need for boilerplate configuration.

---

### 31. **What are the key differences in setting up a project with and without Spring Boot?**

- **With Spring Boot**:

  - Auto-configuration reduces manual setup.
  - Uses starter dependencies for quick setup.
  - Embedded server included.
  - Less boilerplate code.

- **Without Spring Boot**:

  - Manual configuration of dependencies, server, and settings.
  - Need to set up `persistence.xml`, `EntityManagerFactory`, etc.
  - More boilerplate code and potential for configuration errors.

---

### 32. **How is dependency management handled differently with and without Spring Boot?**

- **With Spring Boot**:

  - Uses parent POM and starter dependencies.
  - Manages compatible versions automatically.
  - Simplifies the `pom.xml` or `build.gradle`.

- **Without Spring Boot**:

  - Manually specify each dependency and version.
  - Greater risk of version conflicts.
  - More complex build files.

---

### 33. **What is the purpose of the `persistence.xml` file when not using Spring Boot?**

`persistence.xml` is used to configure JPA settings without Spring Boot:

- Defines the persistence unit.
- Specifies the JPA provider, entities, and database connection details.
- Sets properties like transaction type and caching.

It's essential for initializing JPA in traditional Java EE applications.

---

### 34. **How is the `EntityManagerFactory` managed without Spring Boot?**

Without Spring Boot, you manually create and manage the `EntityManagerFactory`:

- Use `Persistence.createEntityManagerFactory("unitName")` to create it.
- Manage its lifecycle explicitly, including closing it when done.
- Often involves more boilerplate code and careful resource management.

---

### 35. **What is the role of a DAO (Data Access Object) in applications not using Spring Data JPA?**

A DAO is responsible for:

- Encapsulating database access logic.
- Providing CRUD operations for entities.
- Managing connections and transactions manually.
- Serving as an abstraction layer between the application and the database.

Without Spring Data JPA, you write DAOs to handle database interactions.

---

### 36. **How is transaction management handled differently with and without Spring Boot?**

- **With Spring Boot**:

  - Uses declarative transactions with `@Transactional`.
  - Transactions are managed automatically by the framework.

- **Without Spring Boot**:

  - Transactions must be managed programmatically.
  - Use `EntityTransaction` to begin, commit, and rollback transactions manually.
  - More boilerplate and potential for errors.

---

### 37. **What are the advantages of using Spring Boot for JPA and Hibernate configuration?**

- **Reduced Configuration**: Auto-configures JPA settings.
- **Less Boilerplate**: Minimizes setup code.
- **Rapid Development**: Quicker to get started and build applications.
- **Consistency**: Ensures consistent configurations across applications.
- **Easier Maintenance**: Centralized and simplified configuration management.

---

### 38. **What are the disadvantages of manually configuring JPA and Hibernate?**

- **Time-Consuming**: More effort to set up and maintain.
- **Error-Prone**: Higher risk of configuration errors.
- **Boilerplate Code**: More repetitive code for setup and resource management.
- **Complexity**: Requires deeper knowledge of JPA and Hibernate internals.

---

### 39. **How does Spring Boot's auto-configuration work for database connections?**

Spring Boot auto-configures the database connection by:

- Detecting the database driver on the classpath.
- Using default settings or properties defined in `application.properties`.
- Automatically creating a `DataSource` bean.
- Configuring `EntityManagerFactory` and `TransactionManager`.

---

### 40. **What is the purpose of the `@Id` annotation in JPA entities?**

`@Id` marks a field as the primary key of the entity. It's essential for:

- Uniquely identifying each entity instance.
- Allowing the JPA provider to manage entity lifecycle and relationships.

---

### 41. **How is the primary key generation strategy specified in JPA entities?**

Using the `@GeneratedValue` annotation with `strategy` parameter:

```java
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;
```

Strategies include:

- `AUTO`
- `IDENTITY`
- `SEQUENCE`
- `TABLE`

---

### 42. **What does the `@GeneratedValue` annotation do in JPA entities?**

`@GeneratedValue` specifies that the primary key should be generated automatically, according to the specified strategy. It tells the JPA provider how to generate unique identifiers for new entities.

---

### 43. **How can you specify custom column names for entity fields in JPA?**

Use the `@Column` annotation with the `name` attribute:

```java
@Column(name = "movie_name")
private String name;
```

This maps the `name` field to the `movie_name` column in the database.

---

### 44. **What is the purpose of the `@NonNull` annotation in repository methods?**

The `@NonNull` annotation indicates that a method parameter or return value cannot be `null`. It's used for:

- **Documentation**: Signaling to developers that `null` is not acceptable.
- **Static Analysis**: Allowing tools to detect potential `null` issues.
- **Runtime Checks**: Optionally enforcing checks at runtime.

---

### 45. **How does Spring Data JPA implement custom finder methods?**

Spring Data JPA uses method name conventions to create queries automatically. By defining methods like `findByName`, Spring generates the necessary queries based on the method name.

---

### 46. **What is the difference between `@Modifying` and `@Query` annotations?**

- **`@Query`**: Defines custom queries for repository methods.
- **`@Modifying`**: Used with `@Query` for write operations (e.g., `UPDATE`, `DELETE`).

`@Modifying` tells Spring Data JPA that the query changes data and requires a transaction.

---

### 47. **How is pagination handled in Spring Data JPA repositories?**

Pagination is handled using the `Pageable` interface and returning a `Page<T>`:

```java
Page<MovieApp> findAll(Pageable pageable);
```

You create a `Pageable` object specifying page size, number, and sorting, and Spring Data JPA handles the pagination automatically.

---

### 48. **What is the purpose of the `@Param` annotation in custom queries?**

`@Param` binds method parameters to named parameters in the `@Query` annotation. It ensures that the method parameter values are correctly substituted into the query.

---

### 49. **How can you perform bulk inserts using Spring Data JPA?**

Use the `saveAll()` method provided by `JpaRepository`:

```java
List<MovieApp> movies = Arrays.asList(movie1, movie2, movie3);
movieRepo.saveAll(movies);
```

This saves multiple entities in a single batch operation.

---

### 50. **What is the difference between `saveAll()` and `save()` methods in `JpaRepository`?**

- **`save()`**: Saves a single entity.
- **`saveAll()`**: Saves a collection of entities.

`saveAll()` is optimized for bulk operations, potentially improving performance.

---

# Conclusion
