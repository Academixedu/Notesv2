https://docs.spring.io/spring-boot/tutorial/first-application/index.html
Frameworks and Libraries Used by Spring Boot  
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


          
Let's go step by step, starting with setting up the project in **Spring Initializer**, and then gradually adding features, including a simple POST request, a basic entity, and a mock service layer.

---

### **Step 1: Spring Boot Starter Setup (Spring Initializer)**

1. **Go to Spring Initializr**: Visit [Spring Initializr](https://start.spring.io/).
2. **Project setup**:
   - **Project**: Maven Project
   - **Language**: Java
   - **Spring Boot version**: 3.x (or latest stable)
   - **Group**: `com.example`
   - **Artifact**: `movieapi`
   - **Dependencies**: Add the following dependencies:
     - Spring Web
     - Spring Data JPA (we will need this later)
     - H2 Database (for later database integration)
3. **Generate the project**: Click **Generate**, download the project, unzip it, and open it in your IDE (IntelliJ IDEA, Eclipse, etc.).

Now you have a basic Spring Boot project. Let's move forward and add functionality step by step.

---

### **Step 2: Basic Project Structure and Application Class**

Your project structure will look something like this:
```
src
 └── main
     ├── java
     │   └── com.example.movieapi
     │       └── MovieApiApplication.java
     └── resources
         └── application.properties
```

In the `src/main/java/com/example/movieapi/MovieApiApplication.java`, you should have the following code:

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

### **Step 3: Add Basic MovieController**

Now, let’s add a simple controller to handle HTTP GET and POST requests.

1. **Create the `MovieController` class**:  
   In `src/main/java/com/example/movieapi/controller/MovieController.java`:

   ```java
   package com.example.movieapi.controller;

   import org.springframework.web.bind.annotation.GetMapping;
   import org.springframework.web.bind.annotation.PostMapping;
   import org.springframework.web.bind.annotation.RequestBody;
   import org.springframework.web.bind.annotation.RestController;

   @RestController
   public class MovieController {

       // Simple GET endpoint to test if the server is running
       @GetMapping("/hello")
       public String sayHello() {
           return "Welcome to the Movie API!";
       }

       // Basic POST request to add movies (mocked for now)
       @PostMapping("/movies")
       public String createMovie(@RequestBody String movie) {
           // Mock behavior: Simply print the movie data and return a success message
           System.out.println("Movie received: " + movie);
           return "Movie added successfully!";
       }
   }
   ```

### **Step 4: Running the Application**

1. **Run the application**: Go to your IDE’s run configuration and run the `MovieApiApplication` class. You should see the application start on port `8080` by default.
   
2. **Test the application**:
   - Open your browser or Postman and visit: `http://localhost:8080/hello`. You should see:
     ```
     Welcome to the Movie API!
     ```

3. **Test the POST endpoint**:
   - Use Postman or curl to send a POST request:
   - URL: `http://localhost:8080/movies`
   - Method: POST
   - Body (raw JSON):
     ```json
     {
       "title": "Inception",
       "description": "A mind-bending thriller"
     }
     ```
   - You should see the movie data printed in the console, and the response:
     ```
     Movie added successfully!
     ```

---

### **Step 5: Add a Basic `Movie` Entity**

Since the controller is accepting raw JSON strings right now, let’s define a proper `Movie` entity class to hold movie data.

1. **Create the `Movie` entity**:  
   In `src/main/java/com/example/movieapi/entity/Movie.java`:

   ```java
   package com.example.movieapi.entity;

   public class Movie {
       private Long id;
       private String title;
       private String description;

       // Constructors
       public Movie() {}

       public Movie(Long id, String title, String description) {
           this.id = id;
           this.title = title;
           this.description = description;
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

       @Override
       public String toString() {
           return "Movie{" +
                   "id=" + id +
                   ", title='" + title + '\'' +
                   ", description='" + description + '\'' +
                   '}';
       }
   }
   ```

### **Step 6: Update the Controller to Use the Movie Entity**

Now that we have the `Movie` entity, let’s modify the controller to handle `Movie` objects instead of raw strings.

1. **Update the `MovieController`**:

   ```java
   package com.example.movieapi.controller;

   import com.example.movieapi.entity.Movie;
   import org.springframework.web.bind.annotation.*;

   @RestController
   public class MovieController {

       // Simple GET endpoint to test if the server is running
       @GetMapping("/hello")
       public String sayHello() {
           return "Welcome to the Movie API!";
       }

       // POST request to add movies using the Movie entity
       @PostMapping("/movies")
       public String createMovie(@RequestBody Movie movie) {
           // Mock behavior: Simply print the movie object and return a success message
           System.out.println("Movie received: " + movie.toString());
           return "Movie added successfully!";
       }
   }
   ```

### **Step 7: Running and Testing the Application Again**

1. **Run the application**: Re-run the `MovieApiApplication` class.
   
2. **Test the POST endpoint** again using Postman or curl:
   - URL: `http://localhost:8080/movies`
   - Method: POST
   - Body (raw JSON):
     ```json
     {
       "id": 1,
       "title": "Inception",
       "description": "A mind-bending thriller"
     }
     ```
   - You should see the following output in the console:
     ```
     Movie received: Movie{id=1, title='Inception', description='A mind-bending thriller'}
     ```

   - Response in Postman:
     ```
     Movie added successfully!
     ```

---

### **Step 8: Mock Service Layer**

For now, let’s mock the service layer instead of directly using a database.

1. **Create a simple `MovieService` class**:  
   In `src/main/java/com/example/movieapi/service/MovieService.java`:

   ```java
   package com.example.movieapi.service;

   import com.example.movieapi.entity.Movie;
   import org.springframework.stereotype.Service;

   @Service
   public class MovieService {

       // Mock saving the movie (in the real app, you would use a database)
       public void saveMovie(Movie movie) {
           // For now, just print the movie
           System.out.println("Saving movie: " + movie.toString());
       }
   }
   ```

2. **Update the `MovieController` to use `MovieService`**:

   ```java
   package com.example.movieapi.controller;

   import com.example.movieapi.entity.Movie;
   import com.example.movieapi.service.MovieService;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.web.bind.annotation.*;

   @RestController
   public class MovieController {

       @Autowired
       private MovieService movieService;

       // Simple GET endpoint to test if the server is running
       @GetMapping("/hello")
       public String sayHello() {
           return "Welcome to the Movie API!";
       }

       // POST request to add movies using the Movie entity
       @PostMapping("/movies")
       public String createMovie(@RequestBody Movie movie) {
           // Call the service to save the movie
           movieService.saveMovie(movie);
           return "Movie added successfully!";
       }
   }
   ```

### **Step 9: Run the Application with the Mock Service Layer**

1. **Run the application**: Re-run the `MovieApiApplication` class.
   
2. **Test the POST endpoint**:
   - The same POST request as before will now pass through the `MovieService`, which mocks the movie-saving functionality.

---

Let's continue building the Movie API step by step by adding database functionality using JPA, integrating a testing framework, and introducing monitoring tools.

### **Step 10: Add JPA and H2 Database Integration**

We’ll now integrate JPA for persistence and use an in-memory H2 database to store movie data.

#### **Step 10.1: Update `Movie` Entity with JPA Annotations**

We’ll add JPA annotations to the `Movie` entity so it can be mapped to a database table.

1. **Update the `Movie` entity**:
   
   In `src/main/java/com/example/movieapi/entity/Movie.java`, add JPA annotations:

   ```java
   package com.example.movieapi.entity;

   import jakarta.persistence.Entity;
   import jakarta.persistence.GeneratedValue;
   import jakarta.persistence.GenerationType;
   import jakarta.persistence.Id;

   @Entity
   public class Movie {

       @Id
       @GeneratedValue(strategy = GenerationType.IDENTITY)
       private Long id;
       private String title;
       private String description;

       // Constructors
       public Movie() {}

       public Movie(String title, String description) {
           this.title = title;
           this.description = description;
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

       @Override
       public String toString() {
           return "Movie{" +
                   "id=" + id +
                   ", title='" + title + '\'' +
                   ", description='" + description + '\'' +
                   '}';
       }
   }
   ```

#### **Step 10.2: Add `MovieRepository` for Database Operations**

We’ll now create a `MovieRepository` to handle database operations. This repository will extend Spring Data JPA’s `JpaRepository` to automatically provide CRUD operations.

1. **Create `MovieRepository`**:

   In `src/main/java/com/example/movieapi/repository/MovieRepository.java`:

   ```java
   package com.example.movieapi.repository;

   import com.example.movieapi.entity.Movie;
   import org.springframework.data.jpa.repository.JpaRepository;
   import org.springframework.stereotype.Repository;

   @Repository
   public interface MovieRepository extends JpaRepository<Movie, Long> {
   }
   ```

#### **Step 10.3: Update `MovieService` to Use the Repository**

Now, update the service layer to interact with the database using `MovieRepository`.

1. **Update `MovieService`**:

   In `src/main/java/com/example/movieapi/service/MovieService.java`:

   ```java
   package com.example.movieapi.service;

   import com.example.movieapi.entity.Movie;
   import com.example.movieapi.repository.MovieRepository;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.stereotype.Service;

   @Service
   public class MovieService {

       @Autowired
       private MovieRepository movieRepository;

       public Movie saveMovie(Movie movie) {
           return movieRepository.save(movie);  // Persist the movie to the DB
       }

       public Movie getMovieById(Long id) {
           return movieRepository.findById(id).orElse(null);  // Fetch movie from the DB
       }
   }
   ```

#### **Step 10.4: Configure H2 Database**

1. **Configure H2 in `application.properties`**:

   In `src/main/resources/application.properties`, configure the H2 database:

   ```properties
   # H2 Database Configuration
   spring.datasource.url=jdbc:h2:mem:moviesdb
   spring.datasource.driverClassName=org.h2.Driver
   spring.datasource.username=sa
   spring.datasource.password=
   spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
   spring.jpa.hibernate.ddl-auto=update
   spring.h2.console.enabled=true
   ```

2. **Enable the H2 Console**:

   You can now access the H2 console in the browser at `http://localhost:8080/h2-console`.

---

### **Step 11: Test the Application with Database Integration**

Let’s re-run the application and test the endpoints to ensure data is now being persisted to the H2 database.

#### **Step 11.1: Run the Application**

1. **Run `MovieApiApplication`**: Re-run the main class.
2. **Test the POST request**: Use Postman to add a movie to the database:

   - **URL**: `http://localhost:8080/movies`
   - **Method**: POST
   - **Body** (JSON):
     ```json
     {
       "title": "Inception",
       "description": "A mind-bending thriller"
     }
     ```

3. **Check H2 Database**:
   - Visit `http://localhost:8080/h2-console`, use the default settings, and connect to the in-memory database.
   - Run the SQL query to check the data:
     ```sql
     SELECT * FROM MOVIE;
     ```

4. **Test the GET request** to retrieve the movie by its ID:
   - **URL**: `http://localhost:8080/movies/1`
   - This should return the movie you added with ID `1`.

---

### **Step 12: Implement Unit Tests for MovieService**

Next, let’s write unit tests for the `MovieService` to ensure the service layer is working correctly.

#### **Step 12.1: Add JUnit and Mockito Dependencies**

Make sure you have the necessary dependencies in your `pom.xml`:

```xml
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-test</artifactId>
   <scope>test</scope>
</dependency>
```

#### **Step 12.2: Create Unit Tests for `MovieService`**

1. **Create a test class** for `MovieService` in `src/test/java/com/example/movieapi/service/MovieServiceTest.java`:

   ```java
   package com.example.movieapi.service;

   import com.example.movieapi.entity.Movie;
   import com.example.movieapi.repository.MovieRepository;
   import org.junit.jupiter.api.BeforeEach;
   import org.junit.jupiter.api.Test;
   import org.mockito.InjectMocks;
   import org.mockito.Mock;
   import org.mockito.MockitoAnnotations;

   import static org.mockito.Mockito.verify;
   import static org.mockito.Mockito.when;
   import static org.junit.jupiter.api.Assertions.*;

   public class MovieServiceTest {

       @InjectMocks
       private MovieService movieService;

       @Mock
       private MovieRepository movieRepository;

       @BeforeEach
       public void setUp() {
           MockitoAnnotations.openMocks(this);
       }

       @Test
       public void testSaveMovie() {
           Movie movie = new Movie("Inception", "A mind-bending thriller");
           when(movieRepository.save(movie)).thenReturn(movie);

           Movie savedMovie = movieService.saveMovie(movie);
           assertEquals("Inception", savedMovie.getTitle());
           verify(movieRepository).save(movie);
       }

       @Test
       public void testGetMovieById() {
           Movie movie = new Movie("Inception", "A mind-bending thriller");
           when(movieRepository.findById(1L)).thenReturn(java.util.Optional.of(movie));

           Movie fetchedMovie = movieService.getMovieById(1L);
           assertEquals("Inception", fetchedMovie.getTitle());
           verify(movieRepository).findById(1L);
       }
   }
   ```

#### **Step 12.3: Run the Tests**

1. **Run the test suite**: In your IDE, run the test class to ensure everything works.
2. **Check test results**: You should see the tests pass successfully.

---

### **Step 13: Add Monitoring with Actuator**

Now that you have implemented persistence and tests, let’s add basic monitoring using Spring Boot Actuator.

#### **Step 13.1: Add Actuator Dependency**

Add the Spring Boot Actuator dependency to your `pom.xml`:

```xml
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

#### **Step 13.2: Configure Actuator in `application.properties`**

Enable basic Actuator endpoints:

```properties
management.endpoints.web.exposure.include=health,info
management.endpoint.health.show-details=always
```

#### **Step 13.3: Test the Actuator Endpoints**

1. **Run the application** and access the Actuator health endpoint:
   - **URL**: `http://localhost:8080/actuator/health`
   - You should see the application's health status:
     ```json
     {
       "status": "UP"
     }
     ```

2. **Check other Actuator endpoints** like `/info` and `/metrics` for more detailed monitoring.

---

### **Step 14: Summary of the Movie API**

By now, you have implemented:
1. **Spring Boot initialization** and basic setup.
2. **POST and GET endpoints** to add and retrieve movies.
3. **Database integration using JPA and H2**.
4. **Unit testing using JUnit and Mockito**.
5. **Monitoring using Actuator**.

# Movie API Development Challenges


1. **Update GET Endpoint**: Modify the existing GET endpoint in `MovieController` to return all movies instead of "Hello, Movie API!".

2. **Implement getMovieById**: Add a method in `MovieService` to get a movie by ID and create a corresponding GET endpoint in `MovieController`.

3. **Create POST Endpoint**: Implement the `createMovie` method in `MovieService` and add a POST endpoint in `MovieController` to create a new movie.

4. **Implement updateMovie**: Add a method in `MovieService` to update a movie and create a corresponding PUT endpoint in `MovieController`.

5. **Implement deleteMovie**: Add a method in `MovieService` to delete a movie and create a corresponding DELETE endpoint in `MovieController`.

6. **Exception Handling**: Create a `ResourceNotFoundException` class and use it in the `getMovieById` method when a movie is not found.

7. **Validation**: Add validation annotations to the `Movie` entity (e.g., `@NotBlank` for title, `@Min` and `@Max` for rating).

8. **Handle Validation**: Update the controller to handle validation errors and return appropriate error responses.

9. **Custom Query - Find by Title**: Add a method in `MovieRepository` to find movies by title (case-insensitive, partial match).

10. **Title Search Endpoint**: Implement a method in `MovieService` to use the new repository method for title search and add a corresponding endpoint in `MovieController`.

11. **Custom Query - Find by Genre**: Add a method in `MovieRepository` to find movies by genre.

12. **Genre Search Endpoint**: Implement a method in `MovieService` to use the new repository method for genre search and add a corresponding endpoint in `MovieController`.

13. **Custom Query - Find by Minimum Rating**: Add a method in `MovieRepository` to find movies with a rating greater than or equal to a given value.

14. **Rating Search Endpoint**: Implement a method in `MovieService` to use the new repository method for rating search and add a corresponding endpoint in `MovieController`.

15. **Custom Query - Find by Release Date Range**: Add a method in `MovieRepository` to find movies released between two dates.

16. **Date Range Search Endpoint**: Implement a method in `MovieService` to use the new repository method for date range search and add a corresponding endpoint in `MovieController`.

17. **Unified Search Endpoint**: Create a single search endpoint in `MovieController` that can handle all search types (title, genre, rating, date range).

18. **Pagination**: Update the "Get All Movies" endpoint to support pagination.

19. **Sorting**: Add sorting capability to the "Get All Movies" endpoint.

20. **CORS Configuration**: Add a basic CORS configuration to allow requests from all origins.

21. **Logging**: Add logging statements to the service methods.

22. **Unit Test - MovieService**: Write a unit test for the `createMovie` method in `MovieService`.

23. **Unit Test - MovieController**: Write a unit test for the POST endpoint in `MovieController`.

24. **Integration Test**: Write an integration test for creating and retrieving a movie.

25. **Data Initialization**: Create a `DataInitializer` class to populate the database with some initial movies on application startup.

26. **API Documentation**: Add Swagger annotations to document the API endpoints.

27. **Custom Validation**: Create a custom validation annotation for the movie genre (e.g., to check against a list of valid genres).

28. **Error Handling**: Implement a global exception handler to standardize error responses.

29. **API Versioning**: Implement basic API versioning (e.g., v1) in the URL path.

30. **Caching**: Add caching to the "Get Movie by ID" endpoint.

31. **Actuator**: Add and configure Spring Boot Actuator for basic application monitoring.

32. **Custom Actuator Endpoint**: Create a custom Actuator endpoint that returns the count of movies in the database.

33. **Request Logging**: Implement a filter to log incoming HTTP requests.

34. **Response Compression**: Enable and configure response compression for the API.

35. **Database Indexing**: Add appropriate indexes to the `Movie` table for improved query performance.

36. **API Rate Limiting**: Implement basic rate limiting for API requests.

37. **Asynchronous Processing**: Convert a synchronous operation (e.g., creating a movie) to an asynchronous one using `@Async`.

38. **Batch Operations**: Implement a batch insert/update operation for movies.

39. **API Metrics**: Add custom metrics to track API usage (e.g., number of movies created, retrieved).

These challenges build upon the existing code and gradually introduce more advanced concepts and features. Students can validate their progress after each step by testing the new functionality they've just added.

---
Search String Ideas:
Certainly! For each assignment, here are **three variations of Google search strings** focusing on the official Spring documentation (`site:spring.io`):

---

### 1. **Update GET Endpoint**:

- **Variation 1**: `spring boot retrieve all entities from repository site:spring.io`
- **Variation 2**: `spring data jpa findall method example site:spring.io`
- **Variation 3**: `spring boot restcontroller getmapping return list site:spring.io`

---

### 2. **Implement getMovieById**:

- **Variation 1**: `spring boot find entity by id example site:spring.io`
- **Variation 2**: `spring data jpa findbyid method site:spring.io`
- **Variation 3**: `spring boot rest getmapping with pathvariable site:spring.io`

---

### 3. **Create POST Endpoint**:

- **Variation 1**: `spring boot restcontroller postmapping example site:spring.io`
- **Variation 2**: `spring boot handling post requests site:spring.io`
- **Variation 3**: `spring boot save entity using postmapping site:spring.io`

---

### 4. **Implement updateMovie**:

- **Variation 1**: `spring boot rest putmapping update entity site:spring.io`
- **Variation 2**: `spring data jpa save method for update site:spring.io`
- **Variation 3**: `spring boot updating records with putmapping site:spring.io`

---

### 5. **Implement deleteMovie**:

- **Variation 1**: `spring boot rest deletemapping example site:spring.io`
- **Variation 2**: `spring data jpa deletebyid method site:spring.io`
- **Variation 3**: `spring boot deleting data with deletemapping site:spring.io`

---

### 6. **Exception Handling**:

- **Variation 1**: `spring boot custom exceptions handling site:spring.io`
- **Variation 2**: `spring boot controller advice exception site:spring.io`
- **Variation 3**: `spring boot rest exception handling best practices site:spring.io`

---

### 7. **Validation**:

- **Variation 1**: `spring boot bean validation annotations site:spring.io`
- **Variation 2**: `spring boot javax validation example site:spring.io`
- **Variation 3**: `spring boot using @valid annotation site:spring.io`

---

### 8. **Handle Validation**:

- **Variation 1**: `spring boot rest controller validation error handling site:spring.io`
- **Variation 2**: `spring boot bindingresult example site:spring.io`
- **Variation 3**: `spring boot customizing validation error responses site:spring.io`

---

### 9. **Custom Query - Find by Title**:

- **Variation 1**: `spring data jpa derived query methods site:spring.io`
- **Variation 2**: `spring data jpa findby containing ignorecase site:spring.io`
- **Variation 3**: `spring boot custom repository methods site:spring.io`

---

### 10. **Title Search Endpoint**:

- **Variation 1**: `spring boot getmapping with requestparam site:spring.io`
- **Variation 2**: `spring boot rest controller search parameter site:spring.io`
- **Variation 3**: `spring boot handling query parameters site:spring.io`

---

### 11. **Custom Query - Find by Genre**:

- **Variation 1**: `spring data jpa findby field example site:spring.io`
- **Variation 2**: `spring data jpa query methods site:spring.io`
- **Variation 3**: `spring boot repository find by genre site:spring.io`

---

### 12. **Genre Search Endpoint**:

- **Variation 1**: `spring boot rest api get with parameters site:spring.io`
- **Variation 2**: `spring boot requestparam annotation example site:spring.io`
- **Variation 3**: `spring boot rest controller search by genre site:spring.io`

---

### 13. **Custom Query - Find by Minimum Rating**:

- **Variation 1**: `spring data jpa findby rating greater than equal site:spring.io`
- **Variation 2**: `spring data jpa comparison queries site:spring.io`
- **Variation 3**: `spring boot repository methods with greater than site:spring.io`

---

### 14. **Rating Search Endpoint**:

- **Variation 1**: `spring boot getmapping with requestparam example site:spring.io`
- **Variation 2**: `spring boot rest api search by minimum rating site:spring.io`
- **Variation 3**: `spring boot handling numerical request parameters site:spring.io`

---

### 15. **Custom Query - Find by Release Date Range**:

- **Variation 1**: `spring data jpa findby date between site:spring.io`
- **Variation 2**: `spring data jpa querying between dates site:spring.io`
- **Variation 3**: `spring boot repository methods for date range site:spring.io`

---

### 16. **Date Range Search Endpoint**:

- **Variation 1**: `spring boot rest controller date range parameters site:spring.io`
- **Variation 2**: `spring boot handling date request parameters site:spring.io`
- **Variation 3**: `spring boot getmapping multiple parameters site:spring.io`

---

### 17. **Unified Search Endpoint**:

- **Variation 1**: `spring boot rest controller optional requestparams site:spring.io`
- **Variation 2**: `spring boot multiple search criteria site:spring.io`
- **Variation 3**: `spring boot dynamic queries with specifications site:spring.io`

---

### 18. **Pagination**:

- **Variation 1**: `spring data jpa pagination pageable site:spring.io`
- **Variation 2**: `spring boot rest api pagination example site:spring.io`
- **Variation 3**: `spring data jpa using page request site:spring.io`

---

### 19. **Sorting**:

- **Variation 1**: `spring data jpa sorting with sort site:spring.io`
- **Variation 2**: `spring boot rest api sorting example site:spring.io`
- **Variation 3**: `spring data jpa pageable sorting site:spring.io`

---

### 20. **CORS Configuration**:

- **Variation 1**: `spring boot enable global cors configuration site:spring.io`
- **Variation 2**: `spring boot @crossorigin annotation site:spring.io`
- **Variation 3**: `spring boot configuring cors mappings site:spring.io`

---

### 21. **Logging**:

- **Variation 1**: `spring boot logging configuration site:spring.io`
- **Variation 2**: `spring boot adding logs to service methods site:spring.io`
- **Variation 3**: `spring boot using logger example site:spring.io`

---

### 22. **Unit Test - MovieService**:

- **Variation 1**: `spring boot testing service layer site:spring.io`
- **Variation 2**: `spring boot mockito service test example site:spring.io`
- **Variation 3**: `spring boot junit test for service methods site:spring.io`

---

### 23. **Unit Test - MovieController**:

- **Variation 1**: `spring boot testing rest controllers site:spring.io`
- **Variation 2**: `spring boot mockmvc example site:spring.io`
- **Variation 3**: `spring boot controller unit test example site:spring.io`

---

### 24. **Integration Test**:

- **Variation 1**: `spring boot integration testing rest api site:spring.io`
- **Variation 2**: `spring boot testing with @springboottest site:spring.io`
- **Variation 3**: `spring boot full stack integration test site:spring.io`

---

### 25. **Data Initialization**:

- **Variation 1**: `spring boot initializing data with commandlinerunner site:spring.io`
- **Variation 2**: `spring boot preload database on startup site:spring.io`
- **Variation 3**: `spring boot applicationrunner example site:spring.io`

---

### 26. **API Documentation**:

- **Variation 1**: `spring boot openapi documentation site:spring.io`
- **Variation 2**: `spring boot swagger integration site:spring.io`
- **Variation 3**: `spring boot rest api documentation with springdoc site:spring.io`

---

### 27. **Custom Validation**:

- **Variation 1**: `spring boot creating custom constraint annotations site:spring.io`
- **Variation 2**: `spring boot custom validator example site:spring.io`
- **Variation 3**: `spring boot bean validation custom constraints site:spring.io`

---

### 28. **Error Handling**:

- **Variation 1**: `spring boot global exception handling with @controlleradvice site:spring.io`
- **Variation 2**: `spring boot rest api error responses site:spring.io`
- **Variation 3**: `spring boot exception handler example site:spring.io`

---

### 29. **API Versioning**:

- **Variation 1**: `spring boot rest api versioning strategies site:spring.io`
- **Variation 2**: `spring boot api versioning in url path site:spring.io`
- **Variation 3**: `spring boot versioning with request mapping site:spring.io`

---

### 30. **Caching**:

- **Variation 1**: `spring boot enabling caching with cacheable site:spring.io`
- **Variation 2**: `spring boot cache configuration example site:spring.io`
- **Variation 3**: `spring boot caching rest api responses site:spring.io`

---

### 31. **Actuator**:

- **Variation 1**: `spring boot actuator endpoints site:spring.io`
- **Variation 2**: `spring boot monitoring with actuator site:spring.io`
- **Variation 3**: `spring boot configuring actuator endpoints site:spring.io`

---

### 32. **Custom Actuator Endpoint**:

- **Variation 1**: `spring boot creating custom actuator endpoints site:spring.io`
- **Variation 2**: `spring boot actuator add custom endpoint site:spring.io`
- **Variation 3**: `spring boot exposing custom metrics with actuator site:spring.io`

---

### 33. **Request Logging**:

- **Variation 1**: `spring boot logging incoming requests filter site:spring.io`
- **Variation 2**: `spring boot http request logging site:spring.io`
- **Variation 3**: `spring boot interceptors for request logging site:spring.io`

---

### 34. **Response Compression**:

- **Variation 1**: `spring boot enabling gzip compression site:spring.io`
- **Variation 2**: `spring boot compressing response data site:spring.io`
- **Variation 3**: `spring boot response compression configuration site:spring.io`

---

### 35. **Database Indexing**:

- **Variation 1**: `spring data jpa adding indexes to entities site:spring.io`
- **Variation 2**: `spring boot jpa @index annotation example site:spring.io`
- **Variation 3**: `spring boot optimizing queries with indexes site:spring.io`

---

### 36. **API Rate Limiting**:

- **Variation 1**: `spring boot implementing rate limiting site:spring.io`
- **Variation 2**: `spring boot api throttling strategies site:spring.io`
- **Variation 3**: `spring boot limiting api requests per user site:spring.io`

---

### 37. **Asynchronous Processing**:

- **Variation 1**: `spring boot using @async annotation site:spring.io`
- **Variation 2**: `spring boot enabling async methods site:spring.io`
- **Variation 3**: `spring boot asynchronous service methods site:spring.io`

---

### 38. **Batch Operations**:

- **Variation 1**: `spring data jpa batch inserts updates site:spring.io`
- **Variation 2**: `spring boot performing batch operations site:spring.io`
- **Variation 3**: `spring boot jpa bulk data operations site:spring.io`

---

### 39. **API Metrics**:

- **Variation 1**: `spring boot micrometer custom metrics site:spring.io`
- **Variation 2**: `spring boot monitoring api usage site:spring.io`
- **Variation 3**: `spring boot adding metrics to rest api site:spring.io`

---


