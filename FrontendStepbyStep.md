# Building a Simple Weather App with Incremental CSS and JSON Handling

In this tutorial, we'll build a simple weather app using React. We'll focus on incrementally adding CSS to style our app and handling different forms of JSON responses: a single-line string, a single object, and an array of objects.

## Prerequisites

- Basic knowledge of React and CSS
- Node.js and npm installed on your machine

## Step 1: Set Up the Project

1. **Create a new React project**:

   ```bash
   npx create-react-app weather-app
   cd weather-app
   ```

2. **Open the project** in your preferred code editor.

## Step 2: Create Basic Components

We'll start by creating the basic structure of our app with minimal styling.

### 1. Create Component Files

In the `src` directory, create a `components` folder and add the following files:

- `Header.js`
- `Footer.js`
- `Sidebar.js`
- `WeatherDisplay.js`

### 2. Implement the Components

**`src/components/Header.js`**:

```jsx
import React from 'react';

const Header = () => (
  <header className="header">
    <h1>Weather App</h1>
  </header>
);

export default Header;
```

**`src/components/Footer.js`**:

```jsx
import React from 'react';

const Footer = () => (
  <footer className="footer">
    <p>&copy; 2024 Weather App. All rights reserved.</p>
  </footer>
);

export default Footer;
```

**`src/components/Sidebar.js`**:

```jsx
import React from 'react';

const Sidebar = ({ selectedCity, setSelectedCity, dataType, setDataType }) => {
  const cities = ['New York', 'London', 'Tokyo', 'Sydney', 'Paris'];
  const dataTypes = ['single-line', 'single-object', 'multiple-objects'];

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

      <h2>Data Types</h2>
      <ul>
        {dataTypes.map(type => (
          <li
            key={type}
            className={type === dataType ? 'selected' : ''}
            onClick={() => setDataType(type)}
          >
            {type}
          </li>
        ))}
      </ul>
    </aside>
  );
};

export default Sidebar;
```

**`src/components/WeatherDisplay.js`**:

```jsx
import React from 'react';

const WeatherDisplay = ({ weatherData, dataType }) => {
  if (!weatherData) {
    return (
      <main className="weather-display">
        <p>Loading weather data...</p>
      </main>
    );
  }

  let content;
  switch (dataType) {
    case 'single-line':
      content = <p>{weatherData}</p>;
      break;
    case 'single-object':
      content = (
        <div>
          <h2>{weatherData.city}</h2>
          <p>Temperature: {weatherData.temperature}°C</p>
          <p>Condition: {weatherData.condition}</p>
          <p>Humidity: {weatherData.humidity}%</p>
          <p>Wind Speed: {weatherData.windSpeed} m/s</p>
        </div>
      );
      break;
    case 'multiple-objects':
      content = (
        <div>
          {weatherData.map(city => (
            <div key={city.city} className="city-weather">
              <h3>{city.city}</h3>
              <p>Temperature: {city.temperature}°C</p>
              <p>Condition: {city.condition}</p>
            </div>
          ))}
        </div>
      );
      break;
    default:
      content = <p>Unknown data type</p>;
  }

  return (
    <main className="weather-display">
      <div className="weather-info">{content}</div>
    </main>
  );
};

export default WeatherDisplay;
```

**`src/App.js`**:

```jsx
import React, { useState, useEffect } from 'react';
import Header from './components/Header';
import Sidebar from './components/Sidebar';
import WeatherDisplay from './components/WeatherDisplay';
import Footer from './components/Footer';
import './App.css';

const App = () => {
  const [selectedCity, setSelectedCity] = useState('New York');
  const [dataType, setDataType] = useState('single-line');
  const [weatherData, setWeatherData] = useState(null);

  // Simulated API calls
  const fetchWeatherSingleLine = async city => {
    await new Promise(resolve => setTimeout(resolve, 500));
    return `${city}: 22°C, Sunny`;
  };

  const fetchWeatherSingleObject = async city => {
    await new Promise(resolve => setTimeout(resolve, 500));
    return {
      city: city,
      temperature: 22,
      condition: 'Sunny',
      humidity: 60,
      windSpeed: 5,
    };
  };

  const fetchWeatherMultipleObjects = async () => {
    await new Promise(resolve => setTimeout(resolve, 500));
    return [
      { city: 'New York', temperature: 22, condition: 'Sunny' },
      { city: 'London', temperature: 15, condition: 'Cloudy' },
      { city: 'Tokyo', temperature: 28, condition: 'Rainy' },
      { city: 'Sydney', temperature: 20, condition: 'Partly Cloudy' },
      { city: 'Paris', temperature: 18, condition: 'Clear' },
    ];
  };

  useEffect(() => {
    const fetchWeather = async () => {
      let data;
      switch (dataType) {
        case 'single-line':
          data = await fetchWeatherSingleLine(selectedCity);
          break;
        case 'single-object':
          data = await fetchWeatherSingleObject(selectedCity);
          break;
        case 'multiple-objects':
          data = await fetchWeatherMultipleObjects();
          break;
        default:
          data = await fetchWeatherSingleLine(selectedCity);
      }
      setWeatherData(data);
    };

    fetchWeather();
  }, [selectedCity, dataType]);

  return (
    <div className="app-container">
      <Header />
      <div className="main-content">
        <Sidebar
          selectedCity={selectedCity}
          setSelectedCity={setSelectedCity}
          dataType={dataType}
          setDataType={setDataType}
        />
        <WeatherDisplay weatherData={weatherData} dataType={dataType} />
      </div>
      <Footer />
    </div>
  );
};

export default App;
```

## Step 3: Incrementally Add CSS Styles

We'll now add CSS incrementally to style our app.

### 1. Basic Styles (`src/App.css`)

First, reset some default styles and set up the main container.

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
```

**Explanation**:

- We remove the default margin and set a default font.
- The `.app-container` uses flexbox to stack the header, main content, and footer vertically.
- The `.main-content` uses flexbox to place the sidebar and weather display side by side.

### 2. Style the Header and Footer

Add styles for the header and footer.

```css
.header,
.footer {
  padding: 1rem;
  color: white;
}

.header {
  background-color: #3b82f6;
}

.footer {
  background-color: #1f2937;
  text-align: center;
}

.header h1,
.footer p {
  margin: 0;
}
```

**Explanation**:

- The header and footer have padding and white text.
- The header has a blue background, and the footer has a dark background.
- We remove default margins from the headings and paragraphs.

### 3. Style the Sidebar

Add styles for the sidebar and its elements.

```css
.sidebar {
  width: 200px;
  padding: 1rem;
  background-color: #f3f4f6;
}

.sidebar h2 {
  margin-top: 0;
}

.sidebar ul {
  list-style: none;
  padding: 0;
}

.sidebar li {
  padding: 0.5rem;
  margin-bottom: 0.25rem;
  cursor: pointer;
  border-radius: 4px;
}

.sidebar li:hover {
  background-color: #e5e7eb;
}

.sidebar li.selected {
  background-color: #d1d5db;
}
```

**Explanation**:

- The sidebar has a fixed width and light gray background.
- We style the headings, remove list styles, and add padding to list items.
- Hover and selected states provide interactivity.

### 4. Style the Weather Display

Add styles for the weather display area.

```css
.weather-display {
  flex: 1;
  padding: 1rem;
  background-color: #f9fafb;
  display: flex;
  align-items: center;
  justify-content: center;
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

.city-weather {
  margin-bottom: 1rem;
}
```

**Explanation**:

- The weather display fills the remaining space and centers its content.
- We style the weather information with appropriate font sizes.
- For multiple city weather, each city's weather info has a bottom margin.

### 5. Add Responsive Design

Make the layout responsive for smaller screens.

```css
@media (max-width: 768px) {
  .main-content {
    flex-direction: column;
  }

  .sidebar {
    width: 100%;
  }
}
```

**Explanation**:

- On screens smaller than 768px, the sidebar and weather display stack vertically.
- The sidebar takes the full width of the screen.

## Step 4: Handle Different JSON Responses

We'll simulate different types of JSON responses and update our components accordingly.

### 1. Single-Line JSON Response

In `App.js`, we have the function `fetchWeatherSingleLine`:

```jsx
const fetchWeatherSingleLine = async city => {
  await new Promise(resolve => setTimeout(resolve, 500));
  return `${city}: 22°C, Sunny`;
};
```

**Explanation**:

- Simulates an API call returning a single-line string with weather info.

### 2. Single-Object JSON Response

The `fetchWeatherSingleObject` function:

```jsx
const fetchWeatherSingleObject = async city => {
  await new Promise(resolve => setTimeout(resolve, 500));
  return {
    city: city,
    temperature: 22,
    condition: 'Sunny',
    humidity: 60,
    windSpeed: 5,
  };
};
```

**Explanation**:

- Simulates an API call returning a single object with detailed weather info.

### 3. Array of Objects JSON Response

The `fetchWeatherMultipleObjects` function:

```jsx
const fetchWeatherMultipleObjects = async () => {
  await new Promise(resolve => setTimeout(resolve, 500));
  return [
    { city: 'New York', temperature: 22, condition: 'Sunny' },
    { city: 'London', temperature: 15, condition: 'Cloudy' },
    { city: 'Tokyo', temperature: 28, condition: 'Rainy' },
    { city: 'Sydney', temperature: 20, condition: 'Partly Cloudy' },
    { city: 'Paris', temperature: 18, condition: 'Clear' },
  ];
};
```

**Explanation**:

- Simulates an API call returning an array of objects, each representing a city's weather.

## Step 5: Update the WeatherDisplay Component

Our `WeatherDisplay` component handles different data types:

```jsx
const WeatherDisplay = ({ weatherData, dataType }) => {
  // ... same as before ...
};
```

**Explanation**:

- Uses a switch statement to render content based on the `dataType`.
- Renders differently for single-line strings, single objects, and arrays of objects.

## Final Code and CSS

Here's the final `App.css` file with all the styles we've added:

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

.header,
.footer {
  padding: 1rem;
  color: white;
}

.header {
  background-color: #3b82f6;
}

.footer {
  background-color: #1f2937;
  text-align: center;
}

.header h1,
.footer p {
  margin: 0;
}

.sidebar {
  width: 200px;
  padding: 1rem;
  background-color: #f3f4f6;
}

.sidebar h2 {
  margin-top: 0;
}

.sidebar ul {
  list-style: none;
  padding: 0;
}

.sidebar li {
  padding: 0.5rem;
  margin-bottom: 0.25rem;
  cursor: pointer;
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
  background-color: #f9fafb;
  display: flex;
  align-items: center;
  justify-content: center;
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

.city-weather {
  margin-bottom: 1rem;
}

@media (max-width: 768px) {
  .main-content {
    flex-direction: column;
  }

  .sidebar {
    width: 100%;
  }
}
```

## Conclusion

In this tutorial, we've:

- **Created a simple React app** with basic components.
- **Incrementally added CSS** to style the app.
- **Handled different JSON responses**, including single-line strings, single objects, and arrays of objects.

This approach allows you to understand how CSS can be added step by step to enhance the look and feel of your app and how to handle various JSON data formats in React.

From here, you can:

- Connect to a real weather API.
- Add more detailed weather information.
- Enhance the app's responsiveness and accessibility.

