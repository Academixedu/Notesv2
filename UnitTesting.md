```markdown
# Testing with Mockito and JUnit in Spring Boot

Testing in Spring Boot can be efficiently done using JUnit for unit testing and Mockito for mocking dependencies. In this tutorial, we will learn how to write test cases for the `MovieService` and `MovieController` classes in the Movie API project.

## Tools and Dependencies

- **JUnit**: The most popular testing framework in Java.
- **Mockito**: A mocking framework to simulate the behavior of real objects in a controlled way.
- **Spring Boot Test**: Provides integration for Spring testing with JUnit.

Add the following dependencies to your `pom.xml` file:

```xml
<dependencies>
    <!-- Other dependencies -->

    <!-- JUnit Jupiter for unit testing -->
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter-api</artifactId>
        <version>5.7.0</version>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter-engine</artifactId>
        <version>5.7.0</version>
        <scope>test</scope>
    </dependency>

    <!-- Mockito for mocking dependencies -->
    <dependency>
        <groupId>org.mockito</groupId>
        <artifactId>mockito-core</artifactId>
        <version>3.7.7</version>
        <scope>test</scope>
    </dependency>

    <!-- Spring Boot Test Starter -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

## 1. Writing Unit Tests for `MovieService`

Let's start by writing unit tests for the `MovieService` class. Since we don’t want to interact with a real database during unit testing, we will mock the `MovieRepository` dependency.

### `MovieServiceTest.java`

```java
package com.example.movieapi.service;

import com.example.movieapi.entity.Movie;
import com.example.movieapi.exception.ResourceNotFoundException;
import com.example.movieapi.repository.MovieRepository;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.MockitoAnnotations;

import java.time.LocalDate;
import java.util.Arrays;
import java.util.List;
import java.util.Optional;

import static org.junit.jupiter.api.Assertions.*;
import static org.mockito.Mockito.*;

class MovieServiceTest {

    @Mock
    private MovieRepository movieRepository;

    @InjectMocks
    private MovieService movieService;

    private Movie movie;

    @BeforeEach
    void setUp() {
        MockitoAnnotations.openMocks(this); // Initialize Mockito annotations
        movie = new Movie("Inception", "A mind-bending thriller", "Christopher Nolan", 
                          "Sci-Fi", 8.8, LocalDate.of(2010, 7, 16));
        movie.setId(1L);
    }

    @Test
    void testGetAllMovies() {
        // Mocking the repository call
        when(movieRepository.findAll()).thenReturn(Arrays.asList(movie));

        // Call the service method
        List<Movie> movies = movieService.getAllMovies();

        // Assertions
        assertNotNull(movies);
        assertEquals(1, movies.size());
        verify(movieRepository, times(1)).findAll();
    }

    @Test
    void testGetMovieById() {
        // Mocking the repository call
        when(movieRepository.findById(1L)).thenReturn(Optional.of(movie));

        // Call the service method
        Optional<Movie> foundMovie = movieService.getMovieById(1L);

        // Assertions
        assertTrue(foundMovie.isPresent());
        assertEquals("Inception", foundMovie.get().getTitle());
        verify(movieRepository, times(1)).findById(1L);
    }

    @Test
    void testGetMovieById_NotFound() {
        // Mocking the repository call
        when(movieRepository.findById(1L)).thenReturn(Optional.empty());

        // Call the service method and expect an exception
        assertThrows(ResourceNotFoundException.class, () -> movieService.getMovieById(1L).orElseThrow(() -> new ResourceNotFoundException("Movie not found")));
        verify(movieRepository, times(1)).findById(1L);
    }

    @Test
    void testCreateMovie() {
        // Mocking the repository save call
        when(movieRepository.save(any(Movie.class))).thenReturn(movie);

        // Call the service method
        Movie savedMovie = movieService.createMovie(movie);

        // Assertions
        assertNotNull(savedMovie);
        assertEquals("Inception", savedMovie.getTitle());
        verify(movieRepository, times(1)).save(any(Movie.class));
    }
    
    @Test
    void testDeleteMovie() {
        // Mocking repository call
        when(movieRepository.existsById(1L)).thenReturn(true);
        doNothing().when(movieRepository).deleteById(1L);

        // Call the service method
        movieService.deleteMovie(1L);

        // Verify
        verify(movieRepository, times(1)).deleteById(1L);
    }

    @Test
    void testDeleteMovie_NotFound() {
        // Mocking repository call
        when(movieRepository.existsById(1L)).thenReturn(false);

        // Call the service method and expect an exception
        assertThrows(ResourceNotFoundException.class, () -> movieService.deleteMovie(1L));
        verify(movieRepository, times(1)).existsById(1L);
    }
}
```

### Explanation:

- **Mocks and InjectMocks**:
  - `@Mock` is used to mock the `MovieRepository`.
  - `@InjectMocks` is used to inject the mocked repository into `MovieService`.
  
- **`setUp` Method**:
  - Initializes Mockito annotations using `MockitoAnnotations.openMocks(this)` and creates a sample `Movie` object before each test.

- **Test Cases**:
  - `testGetAllMovies()`: Verifies that `findAll()` returns the expected result.
  - `testGetMovieById()`: Verifies that `findById()` returns the correct movie.
  - `testGetMovieById_NotFound()`: Tests that the service throws an exception when the movie is not found.
  - `testCreateMovie()`: Verifies that the movie is correctly saved.
  - `testDeleteMovie()`: Verifies that `deleteById()` is called when the movie exists.
  - `testDeleteMovie_NotFound()`: Verifies that `ResourceNotFoundException` is thrown if the movie to delete is not found.

## 2. Writing Unit Tests for `MovieController`

For `MovieController`, we will mock the `MovieService` dependency and test the HTTP endpoints.

### `MovieControllerTest.java`

```java
package com.example.movieapi.controller;

import com.example.movieapi.entity.Movie;
import com.example.movieapi.service.MovieService;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.MockitoAnnotations;
import org.springframework.http.MediaType;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.setup.MockMvcBuilders;

import java.time.LocalDate;
import java.util.Arrays;
import java.util.Optional;

import static org.mockito.Mockito.*;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.*;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.jsonPath;

class MovieControllerTest {

    @Mock
    private MovieService movieService;

    @InjectMocks
    private MovieController movieController;

    private MockMvc mockMvc;
    private Movie movie;

    @BeforeEach
    void setUp() {
        MockitoAnnotations.openMocks(this); // Initialize Mockito annotations
        mockMvc = MockMvcBuilders.standaloneSetup(movieController).build(); // Initialize MockMvc
        movie = new Movie("Inception", "A mind-bending thriller", "Christopher Nolan", 
                          "Sci-Fi", 8.8, LocalDate.of(2010, 7, 16));
        movie.setId(1L);
    }

    @Test
    void testGetAllMovies() throws Exception {
        // Mock the service call
        when(movieService.getAllMovies()).thenReturn(Arrays.asList(movie));

        // Perform the GET request and verify the response
        mockMvc.perform(get("/api/movies"))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$[0].title").value("Inception"))
                .andExpect(jsonPath("$[0].director").value("Christopher Nolan"));

        verify(movieService, times(1)).getAllMovies();
    }

    @Test
    void testGetMovieById() throws Exception {
        // Mock the service call
        when(movieService.getMovieById(1L)).thenReturn(Optional.of(movie));

        // Perform the GET request and verify the response
        mockMvc.perform(get("/api/movies/1"))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.title").value("Inception"))
                .andExpect(jsonPath("$.director").value("Christopher Nolan"));

        verify(movieService, times(1)).getMovieById(1L);
    }

    @Test
    void testGetMovieById_NotFound() throws Exception {
        // Mock the service call
        when(movieService.getMovieById(1L)).thenReturn(Optional.empty());

        // Perform the GET request and expect a 404 Not Found
        mock

Mvc.perform(get("/api/movies/1"))
                .andExpect(status().isNotFound());

        verify(movieService, times(1)).getMovieById(1L);
    }

    @Test
    void testCreateMovie() throws Exception {
        // Mock the service call
        when(movieService.createMovie(any(Movie.class))).thenReturn(movie);

        // JSON payload for the POST request
        String movieJson = "{ \"title\": \"Inception\", \"description\": \"A mind-bending thriller\", " +
                           "\"director\": \"Christopher Nolan\", \"genre\": \"Sci-Fi\", \"rating\": 8.8, \"releaseDate\": \"2010-07-16\" }";

        // Perform the POST request and verify the response
        mockMvc.perform(post("/api/movies")
                .contentType(MediaType.APPLICATION_JSON)
                .content(movieJson))
                .andExpect(status().isCreated())
                .andExpect(jsonPath("$.title").value("Inception"))
                .andExpect(jsonPath("$.director").value("Christopher Nolan"));

        verify(movieService, times(1)).createMovie(any(Movie.class));
    }

    @Test
    void testDeleteMovie() throws Exception {
        // Mock the service call
        doNothing().when(movieService).deleteMovie(1L);

        // Perform the DELETE request
        mockMvc.perform(delete("/api/movies/1"))
                .andExpect(status().isNoContent());

        verify(movieService, times(1)).deleteMovie(1L);
    }

    @Test
    void testDeleteMovie_NotFound() throws Exception {
        // Mock the service call
        doThrow(new RuntimeException("Movie not found")).when(movieService).deleteMovie(1L);

        // Perform the DELETE request and expect a 404 Not Found
        mockMvc.perform(delete("/api/movies/1"))
                .andExpect(status().isNotFound());

        verify(movieService, times(1)).deleteMovie(1L);
    }
}
```

### Explanation:

- **MockMvc**:
  - `MockMvc` is used to simulate HTTP requests and responses in unit tests.
  - `MockMvcBuilders.standaloneSetup(movieController)` initializes `MockMvc` with the controller under test.

- **Mocking the Service Layer**:
  - `movieService` is mocked using `@Mock` and injected into `movieController` with `@InjectMocks`.

- **Test Cases**:
  - `testGetAllMovies()`: Tests the `GET /api/movies` endpoint and verifies that the correct JSON data is returned.
  - `testGetMovieById()`: Tests the `GET /api/movies/{id}` endpoint and verifies that the movie details are returned.
  - `testGetMovieById_NotFound()`: Simulates the scenario where a movie is not found and verifies that a 404 status code is returned.
  - `testCreateMovie()`: Tests the `POST /api/movies` endpoint and verifies that a new movie is successfully created.
  - `testDeleteMovie()`: Tests the `DELETE /api/movies/{id}` endpoint and verifies that the movie is deleted.
  - `testDeleteMovie_NotFound()`: Simulates the scenario where the movie to be deleted is not found and verifies that a 404 status code is returned.

## 3. Running the Tests

To run the tests, use your IDE or Maven. For Maven, run:

```bash
mvn test
```

## Conclusion

In this tutorial, we created unit tests using JUnit and Mockito to cover various scenarios for `MovieService` and `MovieController`. We demonstrated how to mock dependencies using Mockito and how to simulate HTTP requests using MockMvc in Spring Boot applications. Writing tests this way ensures the functionality of our business logic and API endpoints without requiring real database or service interactions.
```
