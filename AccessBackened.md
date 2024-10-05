# React and Axios Tutorial: Fetching Data from a Backend

## Step 1: Create a new React application

1. Open your terminal
2. Run the following commands:
   ```
   npx create-react-app movie-app
   cd movie-app
   ```

## Step 2: Install Axios

In the project directory, install Axios:

```
npm install axios
```

## Step 3: Create the MovieList component

1. In the `src` folder, create a new file called `MovieList.js`
2. Add the following code to `MovieList.js`:

```jsx
import React, { useState, useEffect } from 'react';
import axios from 'axios';

function MovieList() {
  const [message, setMessage] = useState('');

  useEffect(() => {
    axios.get('http://localhost:8080/hello')
      .then(response => {
        setMessage(response.data);
      })
      .catch(error => {
        console.error('Error fetching data:', error);
        setMessage('Error fetching data');
      });
  }, []);

  return (
    <div>
      <h1>Movie List</h1>
      <p>{message}</p>
    </div>
  );
}

export default MovieList;
```

## Step 4: Update App.js

Replace the contents of `src/App.js` with:

```jsx
import React from 'react';
import MovieList from './MovieList';

function App() {
  return (
    <div className="App">
      <MovieList />
    </div>
  );
}

export default App;
```

## Step 5: Ensure your backend is running

Make sure your Spring Boot backend is running on `localhost:8080` with the following endpoint:

```java
@GetMapping("/hello")
public String sayHello() {
    return "Welcome to the Movie API!";
}
```

## Step 6: Run your React application

In the terminal, in your project directory, run:

```
npm start
```

## Step 7: View your application

Open a web browser and go to `http://localhost:3000`. You should see your Movie List page with the greeting message fetched from your backend.

## Explanation

- The `MovieList` component uses the `useState` hook to manage the state of the `message`.
- The `useEffect` hook is used to perform the Axios GET request when the component mounts.
- Axios sends a GET request to `http://localhost:8080/hello`.
- Upon successful response, the data is stored in the `message` state.
- If there's an error, it's logged to the console and an error message is displayed.
- The `App` component renders the `MovieList` component.

## Troubleshooting

- If you see "Error fetching data", check if your backend server is running.
- Ensure your backend is running on `localhost:8080`. If it's on a different port, update the URL in the Axios request.
- Check your browser's console for detailed error messages.

## Next Steps

- Modify the backend to return more complex data (e.g., a list of movies).
- Update the React component to display this more complex data.
- Add more interactivity, like a form to add new movies or a search feature.
