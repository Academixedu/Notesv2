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

# Movie API Development Challenges

1. **Review Existing Code**: Familiarize yourself with the existing `MovieApiApplication`, `Movie` entity, `MovieRepository`, `MovieService`, and `MovieController` classes.

2. **Update GET Endpoint**: Modify the existing GET endpoint in `MovieController` to return all movies instead of "Hello, Movie API!".

3. **Implement getMovieById**: Add a method in `MovieService` to get a movie by ID and create a corresponding GET endpoint in `MovieController`.

4. **Create POST Endpoint**: Implement the `createMovie` method in `MovieService` and add a POST endpoint in `MovieController` to create a new movie.

5. **Implement updateMovie**: Add a method in `MovieService` to update a movie and create a corresponding PUT endpoint in `MovieController`.

6. **Implement deleteMovie**: Add a method in `MovieService` to delete a movie and create a corresponding DELETE endpoint in `MovieController`.

7. **Exception Handling**: Create a `ResourceNotFoundException` class and use it in the `getMovieById` method when a movie is not found.

8. **Validation**: Add validation annotations to the `Movie` entity (e.g., `@NotBlank` for title, `@Min` and `@Max` for rating).

9. **Handle Validation**: Update the controller to handle validation errors and return appropriate error responses.

10. **Custom Query - Find by Title**: Add a method in `MovieRepository` to find movies by title (case-insensitive, partial match).

11. **Title Search Endpoint**: Implement a method in `MovieService` to use the new repository method for title search and add a corresponding endpoint in `MovieController`.

12. **Custom Query - Find by Genre**: Add a method in `MovieRepository` to find movies by genre.

13. **Genre Search Endpoint**: Implement a method in `MovieService` to use the new repository method for genre search and add a corresponding endpoint in `MovieController`.

14. **Custom Query - Find by Minimum Rating**: Add a method in `MovieRepository` to find movies with a rating greater than or equal to a given value.

15. **Rating Search Endpoint**: Implement a method in `MovieService` to use the new repository method for rating search and add a corresponding endpoint in `MovieController`.

16. **Custom Query - Find by Release Date Range**: Add a method in `MovieRepository` to find movies released between two dates.

17. **Date Range Search Endpoint**: Implement a method in `MovieService` to use the new repository method for date range search and add a corresponding endpoint in `MovieController`.

18. **Unified Search Endpoint**: Create a single search endpoint in `MovieController` that can handle all search types (title, genre, rating, date range).

19. **Pagination**: Update the "Get All Movies" endpoint to support pagination.

20. **Sorting**: Add sorting capability to the "Get All Movies" endpoint.

21. **CORS Configuration**: Add a basic CORS configuration to allow requests from all origins.

22. **Logging**: Add logging statements to the service methods.

23. **Unit Test - MovieService**: Write a unit test for the `createMovie` method in `MovieService`.

24. **Unit Test - MovieController**: Write a unit test for the POST endpoint in `MovieController`.

25. **Integration Test**: Write an integration test for creating and retrieving a movie.

26. **Data Initialization**: Create a `DataInitializer` class to populate the database with some initial movies on application startup.

27. **API Documentation**: Add Swagger annotations to document the API endpoints.

28. **Custom Validation**: Create a custom validation annotation for the movie genre (e.g., to check against a list of valid genres).

29. **Error Handling**: Implement a global exception handler to standardize error responses.

30. **API Versioning**: Implement basic API versioning (e.g., v1) in the URL path.

31. **Caching**: Add caching to the "Get Movie by ID" endpoint.

32. **Actuator**: Add and configure Spring Boot Actuator for basic application monitoring.

33. **Custom Actuator Endpoint**: Create a custom Actuator endpoint that returns the count of movies in the database.

34. **Request Logging**: Implement a filter to log incoming HTTP requests.

35. **Response Compression**: Enable and configure response compression for the API.

36. **Database Indexing**: Add appropriate indexes to the `Movie` table for improved query performance.

37. **API Rate Limiting**: Implement basic rate limiting for API requests.

38. **Asynchronous Processing**: Convert a synchronous operation (e.g., creating a movie) to an asynchronous one using `@Async`.

39. **Batch Operations**: Implement a batch insert/update operation for movies.

40. **API Metrics**: Add custom metrics to track API usage (e.g., number of movies created, retrieved).

These challenges build upon the existing code and gradually introduce more advanced concepts and features. Students can validate their progress after each step by testing the new functionality they've just added.

---


