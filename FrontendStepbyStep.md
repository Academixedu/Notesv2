## Prerequisites
- Basic knowledge of React and CSS
- Node.js and npm installed on your machine

## Step 1: Set up the project

1. Create a new React project:
   ```
   npx create-react-app weather-app
   cd weather-app
   ```

2. Open the project in your code editor.

## Step 2: Create component files

Create the following files in the `src/components` directory:
- `Header.js`
- `Footer.js`
- `Sidebar.js`
- `WeatherDisplay.js`

## Step 3: Implement basic components

Let's start with basic components and minimal styling.

1. `src/components/Header.js`:

```jsx
import React from 'react';

const Header = () => (
  <header className="header">
    <h1>Weather App</h1>
  </header>
);

export default Header;
```

2. `src/components/Footer.js`:

```jsx
import React from 'react';

const Footer = () => (
  <footer className="footer">
    <p>&copy; 2024 Weather App. All rights reserved.</p>
  </footer>
);

export default Footer;
```

3. `src/components/Sidebar.js`:

```jsx
import React from 'react';

const Sidebar = ({ selectedCity, setSelectedCity }) => {
  const cities = ['New York', 'London', 'Tokyo', 'Sydney', 'Paris'];
  return (
    <aside className="sidebar">
      <h2>Cities</h2>
      <ul>
        {cities.map(city => (
          <li
            key={city}
            className={city === selectedCity ? 'selected' : ''}
            onClick={() => setSelectedCity(city)}
          >
            {city}
          </li>
        ))}
      </ul>
    </aside>
  );
};

export default Sidebar;
```

4. `src/components/WeatherDisplay.js`:

```jsx
import React from 'react';

const WeatherDisplay = ({ weatherData }) => (
  <main className="weather-display">
    {weatherData ? (
      <div className="weather-info">
        <h2>{weatherData.city}</h2>
        <p>{weatherData.temperature}°C</p>
        <p>{weatherData.condition}</p>
      </div>
    ) : (
      <p>Loading weather data...</p>
    )}
  </main>
);

export default WeatherDisplay;
```

5. Update `src/App.js`:

```jsx
import React, { useState, useEffect } from 'react';
import Header from './components/Header';
import Sidebar from './components/Sidebar';
import WeatherDisplay from './components/WeatherDisplay';
import Footer from './components/Footer';
import './App.css';

const App = () => {
  const [selectedCity, setSelectedCity] = useState('New York');
  const [weatherData, setWeatherData] = useState(null);

  useEffect(() => {
    const fetchWeather = async () => {
      // Simulate API call
      await new Promise(resolve => setTimeout(resolve, 500));
      setWeatherData({
        city: selectedCity,
        temperature: Math.floor(Math.random() * 30) + 10,
        condition: ['Sunny', 'Cloudy', 'Rainy'][Math.floor(Math.random() * 3)]
      });
    };

    fetchWeather();
  }, [selectedCity]);

  return (
    <div className="app-container">
      <Header />
      <div className="main-content">
        <Sidebar selectedCity={selectedCity} setSelectedCity={setSelectedCity} />
        <WeatherDisplay weatherData={weatherData} />
      </div>
      <Footer />
    </div>
  );
};

export default App;
```

6. Basic CSS in `src/App.css`:

```css
body {
  margin: 0;
  font-family: Arial, sans-serif;
}

.app-container {
  min-height: 100vh;
}
```

Explanation:
- We've set up basic components with minimal structure.
- The CSS resets the body margin and sets a default font.
- We ensure the app container takes at least the full viewport height.

## Step 4: Add layout structure

Let's add CSS to create the basic layout structure.

Update `src/App.css`:

```css
body {
  margin: 0;
  font-family: Arial, sans-serif;
}

.app-container {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.main-content {
  display: flex;
  flex: 1;
}

.header, .footer {
  padding: 1rem;
}

.sidebar {
  width: 200px;
  padding: 1rem;
}

.weather-display {
  flex: 1;
  padding: 1rem;
}
```

Explanation:
- We use flexbox for layout. The `app-container` is a column flex container, ensuring header, main content, and footer stack vertically.
- `main-content` is also a flex container, allowing sidebar and weather display to sit side-by-side.
- `flex: 1` on `main-content` makes it grow to fill available space between header and footer.
- The sidebar has a fixed width, while the weather display grows to fill remaining space (`flex: 1`).
- Padding is added to all sections for spacing.

## Step 5: Style the header and footer

Update `src/App.css`:

```css
/* ... previous styles ... */

.header {
  background-color: #3b82f6;
  color: white;
}

.header h1 {
  margin: 0;
  font-size: 1.5rem;
}

.footer {
  background-color: #1f2937;
  color: white;
  text-align: center;
}
```

Explanation:
- The header gets a blue background with white text.
- We remove default margin from the h1 and set a specific font size.
- The footer has a dark background, white text, and centered content.

## Step 6: Style the sidebar

Update `src/App.css`:

```css
/* ... previous styles ... */

.sidebar {
  width: 200px;
  padding: 1rem;
  background-color: #f3f4f6;
  overflow-y: auto;
}

.sidebar h2 {
  margin-top: 0;
  font-size: 1.2rem;
}

.sidebar ul {
  list-style-type: none;
  padding: 0;
}

.sidebar li {
  cursor: pointer;
  padding: 0.5rem;
  margin-bottom: 0.25rem;
  border-radius: 4px;
}

.sidebar li:hover {
  background-color: #e5e7eb;
}

.sidebar li.selected {
  background-color: #d1d5db;
}
```

Explanation:
- The sidebar gets a light gray background.
- `overflow-y: auto` allows scrolling if the city list is too long.
- We remove default list styling and add custom styles for list items.
- Hover and selected states are added to list items for better interactivity.

## Step 7: Style the weather display

Update `src/App.css`:

```css
/* ... previous styles ... */

.weather-display {
  flex: 1;
  padding: 1rem;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #f9fafb;
}

.weather-info {
  text-align: center;
}

.weather-info h2 {
  font-size: 2rem;
  margin-bottom: 1rem;
}

.weather-info p {
  font-size: 1.2rem;
  margin: 0.5rem 0;
}
```

Explanation:
- The weather display uses flexbox to center its content both vertically and horizontally.
- We add a very light background color for contrast with the sidebar.
- Weather information is centered with appropriate font sizes and spacing.

## Step 8: Add responsive design

Add these media queries to `src/App.css`:

```css
/* ... previous styles ... */

@media (max-width: 768px) {
  .main-content {
    flex-direction: column;
  }

  .sidebar {
    width: 100%;
    max-height: 200px;
  }

  .weather-display {
    min-height: 300px;
  }
}
```

Explanation:
- For screens smaller than 768px wide, we stack the sidebar and weather display vertically.
- The sidebar becomes full-width but with a max height, allowing vertical scrolling for the city list.
- We ensure the weather display has a minimum height for better mobile viewing.

## Final `App.css`

Here's the complete `App.css` file:

```css
body {
  margin: 0;
  font-family: Arial, sans-serif;
}

.app-container {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.main-content {
  display: flex;
  flex: 1;
}

.header, .footer {
  padding: 1rem;
}

.header {
  background-color: #3b82f6;
  color: white;
}

.header h1 {
  margin: 0;
  font-size: 1.5rem;
}

.footer {
  background-color: #1f2937;
  color: white;
  text-align: center;
}

.sidebar {
  width: 200px;
  padding: 1rem;
  background-color: #f3f4f6;
  overflow-y: auto;
}

.sidebar h2 {
  margin-top: 0;
  font-size: 1.2rem;
}

.sidebar ul {
  list-style-type: none;
  padding: 0;
}

.sidebar li {
  cursor: pointer;
  padding: 0.5rem;
  margin-bottom: 0.25rem;
  border-radius: 4px;
}

.sidebar li:hover {
  background-color: #e5e7eb;
}

.sidebar li.selected {
  background-color: #d1d5db;
}

.weather-display {
  flex: 1;
  padding: 1rem;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #f9fafb;
}

.weather-info {
  text-align: center;
}

.weather-info h2 {
  font-size: 2rem;
  margin-bottom: 1rem;
}

.weather-info p {
  font-size: 1.2rem;
  margin: 0.5rem 0;
}

@media (max-width: 768px) {
  .main-content {
    flex-direction: column;
  }

  .sidebar {
    width: 100%;
    max-height: 200px;
  }

  .weather-display {
    min-height: 300px;
  }
}
```

## Conclusion

This tutorial has guided you through creating a weather app with React, focusing on incremental CSS styling. We've covered:

1. Setting up the basic React components
2. Creating a flexible layout with CSS Flexbox
3. Styling individual components (header, footer, sidebar, weather display)
4. Adding responsive design for mobile devices

The resulting app has a clean, responsive design that works well on both desktop and mobile devices. From here, you could extend the app by:

- Connecting to a real weather API
- Adding more weather details (e.g., humidity, wind speed)
- Implementing a search function for cities
- Adding animations for smoother transitions

