https://github.com/gonchigars/batch8backend.git

# Spring Boot Concepts in the Movie Application

1. **@SpringBootApplication**: This annotation is used in the `DemoApplication` class. It combines @Configuration, @EnableAutoConfiguration, and @ComponentScan, setting up Spring Boot's auto-configuration.

2. **Dependency Injection (DI)**: Evident in the `controller` and `movieserives` classes. Constructor-based DI is used, allowing Spring to inject dependencies automatically.

   Example:

   ```java
   public controller(movieserives ms) {
       this.ms = ms;
   }
   ```

3. **@RestController**: Used in the `controller` class, indicating that it's a REST API controller. Spring automatically handles the serialization of returned objects to JSON.

4. **@Service**: Applied to the `movieserives` class, marking it as a service component in Spring's component scanning.

5. **@Repository**: Used on the `movierepo` interface, indicating it's a data access component.

6. **@Entity**: Applied to the `movieapp` class, marking it as a JPA entity to be managed by Spring Data JPA.

7. **@Table**: Used in `movieapp` to specify the database table name.

8. **Spring Data JPA**: The `movierepo` interface extends `JpaRepository`, providing CRUD operations and custom query methods.

9. **@Query**: Used in `movierepo` for custom database queries.

10. **@Transactional**: Applied to methods in `movieserives` and `movierepo` to ensure database transaction integrity.

11. **@Configuration**: Used in `WebConfig` to define a configuration class for CORS settings.

These Spring Boot concepts work together to create a robust, maintainable, and easily testable application structure.

# Relationship: Spring Boot, Spring Data JPA, and Jakarta EE

1. Spring Boot
   |
   ├── Provides auto-configuration and simplifies setup
   |
   v
2. Spring Data JPA
   |
   ├── Simplifies data access layer implementation
   ├── Provides Repository abstractions
   |
   v
3. JPA (Jakarta Persistence API)
   |
   ├── Defines the standard for ORM in Java
   ├── Includes annotations like @Entity, @Id, etc.
   |
   v
4. Hibernate (or another JPA Provider)
   |
   ├── Implements the JPA standard
   ├── Performs actual ORM operations
   |
   v
5. Database (e.g., H2, MySQL, PostgreSQL)

Key Points:

- Jakarta EE (formerly Java EE) provides the specifications, including Jakarta Persistence (JPA).
- Spring Data JPA is built on top of JPA, providing additional conveniences.
- Hibernate is a popular implementation of the JPA specification.
- Spring Boot autoconfigures all these components to work together seamlessly.
- Spring Boot also provides a convenient way to manage the persistence layer.

# Configuring JPA and Hibernate without Spring Boot

## 1. Dependencies

First, you need to manually add the required dependencies to your `pom.xml` (if using Maven) or `build.gradle` (if using Gradle).

For Maven (`pom.xml`):

```xml
<dependencies>
    <!-- JPA API -->
    <dependency>
        <groupId>jakarta.persistence</groupId>
        <artifactId>jakarta.persistence-api</artifactId>
        <version>3.1.0</version>
    </dependency>

    <!-- Hibernate Core (JPA Provider) -->
    <dependency>
        <groupId>org.hibernate.orm</groupId>
        <artifactId>hibernate-core</artifactId>
        <version>6.2.0.Final</version>
    </dependency>

    <!-- Database Driver (e.g., H2) -->
    <dependency>
        <groupId>com.h2database</groupId>
        <artifactId>h2</artifactId>
        <version>2.1.214</version>
    </dependency>
</dependencies>
```

## 2. JPA Configuration

Create a `META-INF/persistence.xml` file in your `src/main/resources` directory:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<persistence version="2.2"
             xmlns="http://xmlns.jcp.org/xml/ns/persistence"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/persistence
                                 http://xmlns.jcp.org/xml/ns/persistence/persistence_2_2.xsd">
    <persistence-unit name="moviePU" transaction-type="RESOURCE_LOCAL">
        <provider>org.hibernate.jpa.HibernatePersistenceProvider</provider>
        <class>com.example.movieapp.entity.Movie</class>
        <properties>
            <property name="jakarta.persistence.jdbc.driver" value="org.h2.Driver"/>
            <property name="jakarta.persistence.jdbc.url" value="jdbc:h2:mem:moviedb"/>
            <property name="jakarta.persistence.jdbc.user" value="sa"/>
            <property name="jakarta.persistence.jdbc.password" value=""/>
            <property name="hibernate.dialect" value="org.hibernate.dialect.H2Dialect"/>
            <property name="hibernate.hbm2ddl.auto" value="update"/>
            <property name="hibernate.show_sql" value="true"/>
        </properties>
    </persistence-unit>
</persistence>
```

## 3. Entity Manager Factory

Create a utility class to manage the EntityManagerFactory:

```java
import jakarta.persistence.EntityManagerFactory;
import jakarta.persistence.Persistence;

public class JPAUtil {
    private static final String PERSISTENCE_UNIT_NAME = "moviePU";
    private static EntityManagerFactory factory;

    public static EntityManagerFactory getEntityManagerFactory() {
        if (factory == null) {
            factory = Persistence.createEntityManagerFactory(PERSISTENCE_UNIT_NAME);
        }
        return factory;
    }

    public static void shutdown() {
        if (factory != null) {
            factory.close();
        }
    }
}
```

## 4. DAO (Data Access Object)

Create a DAO to handle database operations:

```java
import jakarta.persistence.EntityManager;
import jakarta.persistence.EntityTransaction;
import com.example.movieapp.entity.Movie;

public class MovieDAO {
    private EntityManager entityManager;

    public MovieDAO() {
        this.entityManager = JPAUtil.getEntityManagerFactory().createEntityManager();
    }

    public void addMovie(Movie movie) {
        EntityTransaction transaction = entityManager.getTransaction();
        try {
            transaction.begin();
            entityManager.persist(movie);
            transaction.commit();
        } catch (Exception e) {
            if (transaction.isActive()) {
                transaction.rollback();
            }
            e.printStackTrace();
        }
    }

    public Movie findMovie(Long id) {
        return entityManager.find(Movie.class, id);
    }

    // Add other CRUD operations as needed

    public void close() {
        entityManager.close();
    }
}
```

## 5. Usage Example

Here's how you might use this setup in your application:

```java
public class MovieApp {
    public static void main(String[] args) {
        MovieDAO movieDAO = new MovieDAO();

        // Add a movie
        Movie movie = new Movie("Inception", "Christopher Nolan");
        movieDAO.addMovie(movie);

        // Find a movie
        Movie foundMovie = movieDAO.findMovie(1L);
        System.out.println("Found movie: " + foundMovie.getTitle());

        // Close resources
        movieDAO.close();
        JPAUtil.shutdown();
    }
}
```

## Key Differences from Spring Boot

1. **Manual Configuration**: You need to manually set up the `persistence.xml` file and manage the `EntityManagerFactory`.

2. **No Automatic Dependency Management**: You have to explicitly define and manage all dependencies.

3. **No Repository Abstraction**: Without Spring Data JPA, you typically create DAOs with explicit persistence logic.

4. **Transaction Management**: You need to manually begin and commit (or rollback) transactions.

5. **Connection Pool**: You might need to configure a connection pool manually for better performance in a production environment.

6. **Integration with Web Layer**: In a web application, you'd need to set up servlets and handle web requests manually or use a framework like JAX-RS.

This setup gives you more control but requires more boilerplate code and careful resource management.

Now, let me summarize the key points about configuring JPA and Hibernate without Spring Boot:

1. **Manual Dependency Management**:
   You need to explicitly add dependencies for JPA API, Hibernate, and your database driver.

2. **Configuration File**:
   A `persistence.xml` file is required to configure the persistence unit, including database connection details and Hibernate properties.

3. **EntityManagerFactory**:
   You need to manually create and manage the EntityManagerFactory, often using a utility class.

4. **Data Access Objects (DAOs)**:
   Instead of Spring Data repositories, you typically create DAOs that use the EntityManager directly to perform database operations.

5. **Transaction Management**:
   You must explicitly begin, commit, and rollback transactions in your code.

6. **Resource Management**:
   Careful management of resources (like closing EntityManagers) becomes your responsibility.

7. **Lack of Autoconfiguration**:
   You need to be explicit about many configurations that Spring Boot would typically handle automatically.

The main differences from a Spring Boot setup are:

- More boilerplate code
- Explicit configuration rather than convention-over-configuration
- Direct use of JPA/Hibernate APIs rather than Spring abstractions
- Manual resource and transaction management

This approach gives you more control but requires more work and deeper understanding of JPA and Hibernate. It's commonly used in Java EE environments or when you need fine-grained control over your persistence layer.

Spring Boot, by contrast, aims to simplify this setup process by providing sensible defaults and autoconfiguration, allowing developers to get started quickly with less boilerplate code.
