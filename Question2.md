# Spring Boot and JPA: Questions and Answers with Code Examples

## 1. What are the three main annotations combined in `@SpringBootApplication`?

`@SpringBootApplication` combines:
1. `@Configuration`
2. `@EnableAutoConfiguration`
3. `@ComponentScan`

```java
@SpringBootApplication
public class MyApplication {
    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }
}
```

## 2. What is the purpose of the `@Configuration` annotation?

`@Configuration` marks a class as a source of bean definitions.

```java
@Configuration
public class AppConfig {
    @Bean
    public MyService myService() {
        return new MyServiceImpl();
    }
}
```

## 3. How does `@EnableAutoConfiguration` contribute to a Spring Boot application?

It automatically configures your application based on dependencies.

```java
@EnableAutoConfiguration
public class MyAutoConfiguredApp {
    // Spring Boot will auto-configure based on classpath dependencies
}
```

## 4. What does the `@ComponentScan` annotation do in a Spring Boot application?

It tells Spring where to look for Spring components.

```java
@ComponentScan(basePackages = "com.example.myapp")
public class MyApp {
    // Spring will scan com.example.myapp and its sub-packages
}
```

## 5. In the `DemoApplication` class, what method is used to start the Spring Boot application?

The `main` method is used to start the application.

```java
@SpringBootApplication
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

## 6. What is Dependency Injection, and how is it implemented in the Controller class?

Dependency Injection is implemented through constructor-based injection.

```java
@RestController
public class MovieController {
    private final MovieService movieService;

    @Autowired
    public MovieController(MovieService movieService) {
        this.movieService = movieService;
    }
}
```

## 7. What are the benefits of using constructor-based dependency injection?

Benefits include immutability, testability, and clear contracts.

## 8. What is the purpose of the `@RestController` annotation?

`@RestController` combines `@Controller` and `@ResponseBody` for RESTful web services.

```java
@RestController
@RequestMapping("/api/movies")
public class MovieController {
    @GetMapping("/{id}")
    public Movie getMovie(@PathVariable Long id) {
        // Method implementation
    }
}
```

## 9. How does `@RestController` differ from `@Controller`?

`@RestController` is specialized for RESTful services, while `@Controller` is used in MVC applications.

## 10. What is the role of the `@Service` annotation in Spring Boot applications?

`@Service` marks a class as a service provider in the business layer.

```java
@Service
public class MovieService {
    // Service methods
}
```

## 11. How does the `@Service` annotation relate to the `@Component` annotation?

`@Service` is a specialization of `@Component` for service layer classes.

## 12. What is the primary function of the `@Repository` annotation?

`@Repository` indicates a data access component and enables exception translation.

```java
@Repository
public interface MovieRepository extends JpaRepository<Movie, Long> {
    // Repository methods
}
```

## 13. How does the `@Repository` annotation handle database exceptions?

It translates database-specific exceptions to Spring's `DataAccessException` hierarchy.

## 14. What does the `@Entity` annotation signify in JPA?

`@Entity` marks a class as a JPA entity, mapped to a database table.

```java
@Entity
public class Movie {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String title;
    // Other fields, getters, and setters
}
```

## 15. How is the `@Table` annotation used in conjunction with `@Entity`?

`@Table` specifies the database table name for an entity.

```java
@Entity
@Table(name = "movies")
public class Movie {
    // Class content
}
```

## 16. What is the purpose of extending `JpaRepository` in Spring Data JPA?

It provides CRUD operations and more out of the box.

```java
public interface MovieRepository extends JpaRepository<Movie, Long> {
    // Custom query methods can be added here
}
```

## 17. How does Spring Data JPA simplify data access layers?

It provides default implementations and supports custom query methods.

## 18. What is the purpose of the `@Query` annotation in Spring Data JPA?

`@Query` allows defining custom queries in repository methods.

```java
public interface MovieRepository extends JpaRepository<Movie, Long> {
    @Query("SELECT m FROM Movie m WHERE m.director = :director")
    List<Movie> findMoviesByDirector(@Param("director") String director);
}
```

## 19. How can you write custom queries using the `@Query` annotation?

You can write JPQL or native SQL queries within the `@Query` annotation.

## 20. What is the difference between JPQL and native SQL in `@Query` annotations?

JPQL is object-oriented and database-independent, while native SQL uses database-specific syntax.

```java
// JPQL
@Query("SELECT m FROM Movie m WHERE m.year > :year")
List<Movie> findMoviesAfterYear(@Param("year") int year);

// Native SQL
@Query(value = "SELECT * FROM movies WHERE year > :year", nativeQuery = true)
List<Movie> findMoviesAfterYearNative(@Param("year") int year);
```

## 21. What does the `@Transactional` annotation do in Spring Boot applications?

It manages transaction boundaries declaratively.

```java
@Service
public class MovieService {
    @Transactional
    public void updateMovie(Movie movie) {
        // Method implementation
    }
}
```

## 22. At what levels can the `@Transactional` annotation be applied?

It can be applied at method or class level.

## 23. How does Spring Boot handle transaction management when using `@Transactional`?

Spring Boot uses AOP proxies to manage transactions.

## 24. What is the purpose of the `@Configuration` annotation in the `WebConfig` class?

It marks the class as a source of bean definitions for configuration.

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    // Configuration methods
}
```

## 25. How is CORS (Cross-Origin Resource Sharing) configured in the `WebConfig` class?

CORS is configured by overriding `addCorsMappings` method.

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/api/**")
                .allowedOrigins("http://localhost:3000")
                .allowedMethods("GET", "POST", "PUT", "DELETE");
    }
}
```

## 26. What is the relationship between Spring Boot and Spring Data JPA?

Spring Boot auto-configures Spring Data JPA, simplifying its setup and use.

## 27. How does Spring Data JPA relate to the Jakarta Persistence API?

Spring Data JPA builds on and extends the Jakarta Persistence API.

## 28. What role does Hibernate play in a Spring Boot application using JPA?

Hibernate is the default JPA provider, implementing JPA specifications.

## 29. In the technology stack, where does the database fit in relation to Hibernate?

The database is at the bottom, with Hibernate acting as a bridge between the application and the database.

## 30. How does Spring Boot simplify the configuration of JPA and Hibernate?

Spring Boot auto-configures JPA and Hibernate based on dependencies and properties.

```properties
# application.properties
spring.datasource.url=jdbc:mysql://localhost:3306/moviedb
spring.datasource.username=root
spring.datasource.password=password
spring.jpa.hibernate.ddl-auto=update
```

## 31. What are the key differences in setting up a project with and without Spring Boot?

Spring Boot reduces manual configuration and provides auto-configuration.

## 32. How is dependency management handled differently with and without Spring Boot?

Spring Boot uses starter dependencies and manages versions automatically.

```xml
<!-- pom.xml with Spring Boot -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

## 33. What is the purpose of the `persistence.xml` file when not using Spring Boot?

`persistence.xml` configures JPA settings in traditional Java EE applications.

## 34. How is the `EntityManagerFactory` managed without Spring Boot?

It's manually created and managed using `Persistence.createEntityManagerFactory()`.

```java
EntityManagerFactory emf = Persistence.createEntityManagerFactory("myPersistenceUnit");
EntityManager em = emf.createEntityManager();
```

## 35. What is the role of a DAO (Data Access Object) in applications not using Spring Data JPA?

DAOs encapsulate database access logic and provide CRUD operations.

```java
public class MovieDao {
    private EntityManager em;

    public Movie findById(Long id) {
        return em.find(Movie.class, id);
    }
    // Other CRUD methods
}
```

## 36. How is transaction management handled differently with and without Spring Boot?

Spring Boot uses declarative transactions, while without it, transactions are often managed programmatically.

```java
// Without Spring Boot
EntityTransaction transaction = em.getTransaction();
try {
    transaction.begin();
    // Perform database operations
    transaction.commit();
} catch (Exception e) {
    transaction.rollback();
}
```

## 37. What are the advantages of using Spring Boot for JPA and Hibernate configuration?

Advantages include reduced configuration, less boilerplate, and faster development.

## 38. What are the disadvantages of manually configuring JPA and Hibernate?

Disadvantages include more time-consuming setup, potential for errors, and more boilerplate code.

## 39. How does Spring Boot's auto-configuration work for database connections?

It detects database drivers and configures based on `application.properties`.

## 40. What is the purpose of the `@Id` annotation in JPA entities?

`@Id` marks a field as the primary key of the entity.

```java
@Entity
public class Movie {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    // Other fields
}
```

## 41. How is the primary key generation strategy specified in JPA entities?

It's specified using the `@GeneratedValue` annotation.

## 42. What does the `@GeneratedValue` annotation do in JPA entities?

It specifies how the primary key should be automatically generated.

## 43. How can you specify custom column names for entity fields in JPA?

Use the `@Column` annotation with the `name` attribute.

```java
@Entity
public class Movie {
    @Column(name = "movie_title")
    private String title;
}
```

## 44. What is the purpose of the `@NonNull` annotation in repository methods?

It indicates that a parameter or return value cannot be null.

```java
public interface MovieRepository extends JpaRepository<Movie, Long> {
    @NonNull
    List<Movie> findByDirector(@NonNull String director);
}
```

## 45. How does Spring Data JPA implement custom finder methods?

It uses method name conventions to generate queries.

```java
public interface MovieRepository extends JpaRepository<Movie, Long> {
    List<Movie> findByYearGreaterThan(int year);
}
```

## 46. What is the difference between `@Modifying` and `@Query` annotations?

`@Query` defines custom queries, while `@Modifying` is used with `@Query` for write operations.

```java
@Modifying
@Query("UPDATE Movie m SET m.title = :title WHERE m.id = :id")
void updateMovieTitle(@Param("id") Long id, @Param("title") String title);
```

## 47. How is pagination handled in Spring Data JPA repositories?

Pagination is handled using the `Pageable` interface.

```java
public interface MovieRepository extends JpaRepository<Movie, Long> {
    Page<Movie> findByDirector(String director, Pageable pageable);
}
```

## 48. What is the purpose of the `@Param` annotation in custom queries?

It binds method parameters to named parameters in the query.

## 49. How can you perform bulk inserts using Spring Data JPA?

Use the `saveAll()` method provided by `JpaRepository`.

```java
List<Movie> movies = Arrays.asList(new Movie("Title1"), new Movie("Title2"));
movieRepository.saveAll(movies);
```

## 50. What is the difference between `saveAll()` and `save()` methods in `JpaRepository`?

`save()` is for single entities, while `saveAll()` is for collections, optimized for bulk operations.
