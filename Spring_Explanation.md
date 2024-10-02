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


