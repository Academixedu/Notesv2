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
