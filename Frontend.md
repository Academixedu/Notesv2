```javascript
// File: src/App.js
import React from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import MovieList from './components/MovieList';
import MovieDetails from './components/MovieDetails';
import AddMovie from './components/AddMovie';
import Navbar from './components/Navbar';

function App() {
  return (
    <Router>
      <div className="App">
        <Navbar />
        <Switch>
          <Route exact path="/" component={MovieList} />
          <Route path="/movie/:id" component={MovieDetails} />
          <Route path="/add" component={AddMovie} />
        </Switch>
      </div>
    </Router>
  );
}

export default App;

// File: src/components/Navbar.js
import React from 'react';
import { Link } from 'react-router-dom';

function Navbar() {
  return (
    <nav>
      <ul>
        <li><Link to="/">Home</Link></li>
        <li><Link to="/add">Add Movie</Link></li>
      </ul>
    </nav>
  );
}

export default Navbar;

// File: src/components/MovieList.js
import React, { useState, useEffect } from 'react';
import { Link } from 'react-router-dom';

function MovieList() {
  const [movies, setMovies] = useState([]);

  useEffect(() => {
    // Fetch movies from API
    fetch('http://localhost:8080/movies')
      .then(response => response.json())
      .then(data => setMovies(data))
      .catch(error => console.error('Error:', error));
  }, []);

  return (
    <div>
      <h1>Movie List</h1>
      <ul>
        {movies.map(movie => (
          <li key={movie.id}>
            <Link to={`/movie/${movie.id}`}>{movie.title}</Link>
          </li>
        ))}
      </ul>
    </div>
  );
}

export default MovieList;

// File: src/components/MovieDetails.js
import React, { useState, useEffect } from 'react';
import { useParams } from 'react-router-dom';

function MovieDetails() {
  const [movie, setMovie] = useState(null);
  const { id } = useParams();

  useEffect(() => {
    // Fetch movie details from API
    fetch(`http://localhost:8080/movies/${id}`)
      .then(response => response.json())
      .then(data => setMovie(data))
      .catch(error => console.error('Error:', error));
  }, [id]);

  if (!movie) return <div>Loading...</div>;

  return (
    <div>
      <h1>{movie.title}</h1>
      <p>{movie.description}</p>
      {/* Add more movie details here */}
    </div>
  );
}

export default MovieDetails;

// File: src/components/AddMovie.js
import React, { useState } from 'react';
import { useHistory } from 'react-router-dom';

function AddMovie() {
  const [title, setTitle] = useState('');
  const [description, setDescription] = useState('');
  const history = useHistory();

  const handleSubmit = (e) => {
    e.preventDefault();
    // Send POST request to API
    fetch('http://localhost:8080/movies', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ title, description }),
    })
      .then(response => response.json())
      .then(() => {
        history.push('/');
      })
      .catch(error => console.error('Error:', error));
  };

  return (
    <div>
      <h1>Add New Movie</h1>
      <form onSubmit={handleSubmit}>
        <input
          type="text"
          placeholder="Title"
          value={title}
          onChange={(e) => setTitle(e.target.value)}
          required
        />
        <textarea
          placeholder="Description"
          value={description}
          onChange={(e) => setDescription(e.target.value)}
          required
        ></textarea>
        <button type="submit">Add Movie</button>
      </form>
    </div>
  );
}

export default AddMovie;

```

Now, let's go through the steps to set up and build this React frontend for your Movie API:

### Step 1: Set up the React project

1. Create a new React project using Create React App:
   ```
   npx create-react-app movie-frontend
   cd movie-frontend
   ```

2. Install additional dependencies:
   ```
   npm install react-router-dom axios
   ```

### Step 2: Set up the basic structure

1. Replace the contents of `src/App.js` with the code provided in the artifact.

2. Create the component files in the `src/components` directory:
   - `Navbar.js`
   - `MovieList.js`
   - `MovieDetails.js`
   - `AddMovie.js`

3. Copy the respective component code from the artifact into each file.

### Step 3: Implement the MovieList component

The `MovieList` component fetches all movies from the API and displays them as a list. Each movie title is a link to its details page.

### Step 4: Implement the MovieDetails component

The `MovieDetails` component fetches and displays details for a specific movie based on the ID in the URL.

### Step 5: Implement the AddMovie component

The `AddMovie` component provides a form to add a new movie. It sends a POST request to the API when the form is submitted.

### Step 6: Implement the Navbar component

The `Navbar` component provides navigation links to the home page and the add movie page.

### Step 7: Style your components

Add some basic CSS to make your app look better. You can create a `src/App.css` file and import it in `App.js`.

there wasn't any specific styling to center the movie list. Let's modify the `MovieList` component and add some basic CSS to center the content. 



```javascript
// File: src/components/MovieList.js
import React, { useState, useEffect } from 'react';
import { Link } from 'react-router-dom';
import './MovieList.css'; // Import the CSS file

function MovieList() {
  const [movies, setMovies] = useState([]);

  useEffect(() => {
    // Fetch movies from API
    fetch('http://localhost:8080/movies')
      .then(response => response.json())
      .then(data => setMovies(data))
      .catch(error => console.error('Error:', error));
  }, []);

  return (
    <div className="movie-list-container">
      <h1>Movie List</h1>
      <ul className="movie-list">
        {movies.map(movie => (
          <li key={movie.id} className="movie-item">
            <Link to={`/movie/${movie.id}`}>{movie.title}</Link>
          </li>
        ))}
      </ul>
    </div>
  );
}

export default MovieList;

// File: src/components/MovieList.css
.movie-list-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
}

.movie-list {
  list-style-type: none;
  padding: 0;
  width: 100%;
  max-width: 600px;
}

.movie-item {
  margin-bottom: 10px;
  padding: 10px;
  background-color: #f0f0f0;
  border-radius: 5px;
}

.movie-item a {
  text-decoration: none;
  color: #333;
  font-weight: bold;
}

.movie-item a:hover {
  color: #007bff;
}

```

To center the movie list and improve its appearance, follow these steps:

1. Create a new CSS file:
   Create a file named `MovieList.css` in the same directory as your `MovieList.js` component.

2. Update the `MovieList.js` component:
   Replace the content of `MovieList.js` with the code provided in the artifact above.

3. Add the CSS:
   Copy the CSS code from the artifact into your `MovieList.css` file.

Here's an explanation of the changes:

1. We've added a new `className` to the main container div: `movie-list-container`. This class is used to center the content.

2. We've also added classes to the `ul` and `li` elements for styling.

3. In the CSS:
   - `.movie-list-container` uses flexbox to center its contents vertically and horizontally.
   - `.movie-list` is given a maximum width and centered using `margin: 0 auto`.
   - `.movie-item` is styled to look like cards with some padding and background color.

4. We've imported the CSS file at the top of the `MovieList.js` component.

These changes will center the movie list on the page and give it a more appealing look. The list items will appear as cards, and the entire list will be centered both horizontally and vertically.

Remember to adjust the styles as needed to fit your overall design aesthetic. You might want to apply similar styling principles to other components in your app for consistency.

### Step 8: Test your application

1. Start your Spring Boot backend application.
2. In the `movie-frontend` directory, run:
   ```
   npm start
   ```
3. Open a web browser and go to `http://localhost:3000` to see your React app.

### Step 9: Add error handling and loading states

Enhance your components to handle loading states and errors when fetching data from the API.

### Step 10: Implement movie editing and deletion

Add functionality to edit and delete movies. This will involve creating new components and adding new API calls.

### Step 11: Implement search functionality

Add a search bar to filter movies based on title, genre, or other criteria.

### Step 12: Add pagination

Implement pagination for the movie list to handle large numbers of movies efficiently.

### Step 13: Improve UI/UX

Enhance the user interface with a better design, possibly using a UI library like Material-UI or Ant Design.

### Step 14: Add authentication

Implement user authentication to secure your app and allow for user-specific features.

### Step 15: Implement state management

As your app grows, consider using a state management library like Redux or MobX for more efficient state handling.

This step-by-step guide provides a solid foundation for building a React frontend for your Movie API. Each step builds upon the previous one, gradually adding more functionality and complexity to your application. Remember to test each feature as you implement it to ensure everything is working correctly.
