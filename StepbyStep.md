https://docs.spring.io/spring-boot/tutorial/first-application/index.html

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

You can now continue by adding more features like pagination, custom queries, security, and more complex business logic, but you already have a fully functioning Movie API!


Here’s a series of 20-30 small challenges, each focusing on a specific aspect of the Movie API, designed to gradually build up the full system. Each challenge is designed to ensure you can validate progress at each step, with minimal complexity.

---

### **Challenge 1: Project Setup**
1. **Objective**: Set up a Spring Boot project.
   - Go to Spring Initializr and create a Maven project with Spring Web as a dependency.
   - Import the project into your IDE.
   - Validate: Ensure the application starts successfully with no errors by running it.

---

### **Challenge 2: Application Entry Point**
2. **Objective**: Add a main class to run the application.
   - Add a class `MovieApiApplication` with the `@SpringBootApplication` annotation and a `main` method.
   - Validate: The application should run on port `8080` and print logs that the Spring Boot application is starting.

---

### **Challenge 3: Basic Controller Setup**
3. **Objective**: Create a simple REST controller.
   - Create a class `MovieController` with a `@RestController` annotation.
   - Add a `@GetMapping("/hello")` method that returns a simple greeting message.
   - Validate: Visit `http://localhost:8080/hello` in the browser, and you should see the greeting message.

---

### **Challenge 4: POST Request with Raw Data**
4. **Objective**: Handle POST requests with raw JSON data.
   - Add a `@PostMapping("/movies")` method that accepts a raw `@RequestBody String`.
   - Validate: Use Postman to send a POST request with a raw JSON string, and log the data to the console.

---

### **Challenge 5: Create Movie Class**
5. **Objective**: Create a simple `Movie` class.
   - Define a `Movie` class with fields: `id`, `title`, and `description`, including getters and setters.
   - Validate: Replace the raw string in the `@PostMapping` method with `@RequestBody Movie` and ensure data is logged correctly.

---

### **Challenge 6: Mock Service Layer**
6. **Objective**: Create a mock `MovieService` class.
   - Add a `MovieService` class with a `saveMovie(Movie movie)` method.
   - Validate: Call this service in the `MovieController` and print the received movie in the service layer.

---

### **Challenge 7: Add Validation to Movie Entity**
7. **Objective**: Add validation annotations.
   - Add `@NotBlank` for the `title` field in `Movie`.
   - Validate: Use `@Valid` in the `@PostMapping` method and try to submit an empty title in Postman to see validation errors.

---

### **Challenge 8: Integrate JPA**
8. **Objective**: Add JPA annotations to the `Movie` class.
   - Add `@Entity`, `@Id`, and `@GeneratedValue` to make the `Movie` class a JPA entity.
   - Validate: Ensure no errors occur on application startup after adding JPA annotations.

---

### **Challenge 9: Create MovieRepository**
9. **Objective**: Create a `MovieRepository` interface.
   - Extend `JpaRepository<Movie, Long>` in `MovieRepository`.
   - Validate: Ensure the application starts without issues and check if the repository is properly injected.

---

### **Challenge 10: Save Movie to Database**
10. **Objective**: Persist movie data in the H2 database.
    - Update `MovieService` to save movies to the database via `MovieRepository`.
    - Validate: Use Postman to submit a movie and check the H2 console (`http://localhost:8080/h2-console`) to verify the data is stored.

---

### **Challenge 11: Create GET Endpoint for Movies**
11. **Objective**: Add a method to get all movies.
    - Create a `@GetMapping("/movies")` method in the controller to retrieve all movies.
    - Validate: Use Postman to GET movies and verify data from the H2 database is returned.

---

### **Challenge 12: Find Movie by ID**
12. **Objective**: Retrieve a movie by its ID.
    - Create a `@GetMapping("/movies/{id}")` method to fetch a movie by its ID.
    - Validate: Use Postman to fetch a specific movie by ID and verify correct data is returned.

---

### **Challenge 13: Handle Resource Not Found**
13. **Objective**: Add exception handling for missing movies.
    - Create a `ResourceNotFoundException` and throw it in the `MovieService` when a movie is not found.
    - Validate: Try to fetch a non-existing movie by ID and see if the proper error message is returned.

---

### **Challenge 14: Add Update Movie Endpoint**
14. **Objective**: Update an existing movie.
    - Create a `@PutMapping("/movies/{id}")` method to update a movie.
    - Validate: Use Postman to update an existing movie and check the H2 console for updated data.

---

### **Challenge 15: Delete Movie by ID**
15. **Objective**: Add a method to delete a movie.
    - Create a `@DeleteMapping("/movies/{id}")` method to delete a movie by ID.
    - Validate: Use Postman to delete a movie, and verify that it's removed from the database.

---

### **Challenge 16: Implement Search by Title**
16. **Objective**: Add a method to search movies by title.
    - Add a `findByTitleContainingIgnoreCase` method in `MovieRepository`.
    - Validate: Use Postman to search for movies by title.

---

### **Challenge 17: Search Movies by Genre**
17. **Objective**: Add a method to search movies by genre.
    - Add a `findByGenre` method in `MovieRepository` and corresponding `@GetMapping` endpoint.
    - Validate: Use Postman to search movies by genre.

---

### **Challenge 18: Search Movies by Rating**
18. **Objective**: Add a method to search movies by rating.
    - Add a `findByRatingGreaterThanEqual` query in `MovieRepository`.
    - Validate: Search for movies with a rating above a certain threshold using Postman.

---

### **Challenge 19: Add Release Date Search**
19. **Objective**: Implement a search by release date range.
    - Add a `findByReleaseDateBetween` query in `MovieRepository` and a corresponding endpoint.
    - Validate: Search movies within a specific date range.

---

### **Challenge 20: Implement Unit Tests for MovieService**
20. **Objective**: Add unit tests using JUnit and Mockito.
    - Write tests for the `saveMovie()` and `getMovieById()` methods in `MovieService`.
    - Validate: Run the tests and ensure they pass.

---

### **Challenge 21: Add Validation for Movie Rating**
21. **Objective**: Add constraints on the movie rating.
    - Add `@Min(0)` and `@Max(10)` for the `rating` field in `Movie`.
    - Validate: Submit invalid ratings using Postman and ensure validation errors are returned.

---

### **Challenge 22: Add Pagination for Movie List**
22. **Objective**: Add pagination to the `GET /movies` endpoint.
    - Modify `MovieRepository` to return paginated results.
    - Validate: Use Postman to retrieve paginated results by adding `page` and `size` query parameters.

---

### **Challenge 23: Enable Actuator for Health Monitoring**
23. **Objective**: Add Spring Boot Actuator.
    - Add the `spring-boot-starter-actuator` dependency and enable health endpoints.
    - Validate: Access the health endpoint via `http://localhost:8080/actuator/health`.

---

### **Challenge 24: Implement Sorting**
24. **Objective**: Add sorting capabilities for movie results.
    - Modify `MovieRepository` to support sorting by `title`, `rating`, etc.
    - Validate: Use Postman to get sorted results by adding sort parameters.

---

### **Challenge 25: Global Exception Handling**
25. **Objective**: Implement global exception handling.
    - Use `@ControllerAdvice` to create a global exception handler for validation errors and `ResourceNotFoundException`.
    - Validate: Submit invalid data and ensure proper error messages are returned.

---

### **Challenge 26: Add CORS Configuration**
26. **Objective**: Enable CORS for API access from other domains.
    - Add a CORS configuration class using `WebMvcConfigurer`.
    - Validate: Ensure the API is accessible from a frontend running on a different domain.

---

### **Challenge 27: Refactor MovieService with Transaction Management**
27. **Objective**: Add transaction management.
    - Annotate the `updateMovie()` method in `MovieService` with `@Transactional`.
    - Validate: Ensure transactional integrity by performing multiple operations in the method.

---

### **Challenge 28: Enable H2 Console**
28. **Objective**: Enable the H2 database console for debugging.
    - Add H2 configuration in `application.properties` to enable the H2 console.
    - Validate: Access the H2 console at `http://localhost:8080/h2-console`.

---

### **Challenge 29: Custom Query for Complex Search**
29. **Objective**: Add a custom query for more complex movie search.
    - Write a custom JPQL query in `MovieRepository` to search for movies based on multiple fields (e.g., title, genre, and rating).
    - Validate: Use Postman to search with multiple criteria.

---

