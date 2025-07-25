# SkyCast - Find My Weather
## Date:25-07-2025
## Objective:
To build a responsive single-page application using React that allows users to enter a city name and retrieve real-time weather information using the OpenWeatherMap API. This project demonstrates the use of Axios for API calls, React Router for navigation, React Hooks for state management, controlled components with validation, and basic styling with CSS.
## Tasks:

#### 1. Project Setup
Initialize React app.

Install necessary dependencies: npm install axios react-router-dom

#### 2. Routing
Set up BrowserRouter in App.js.

Create two routes:

/ – Home page with input form.

/weather – Page to display weather results.

#### 3. Home Page (City Input)
Create a controlled input field for the city name.

Add validation to ensure the input is not empty.

On valid form submission, navigate to /weather and store the city name.

#### 4. Weather Page (API Integration)
Use Axios to fetch data from the OpenWeatherMap API using the city name.

Show temperature, humidity, wind speed, and weather condition.

Convert and display temperature in both Celsius and Fahrenheit using useMemo.

#### 5. React Hooks
Use useState for managing city, weather data, and loading state.

Use useEffect to trigger the Axios call on page load.

Use useCallback to optimize form submit handler.

Use useMemo for temperature conversion logic.

#### 6. UI Styling (CSS)
Create a responsive and clean layout using CSS.

Style form, buttons, weather display cards, and navigation links.

## Programs:
Weather.jsx

```
import { useEffect, useState, useMemo } from 'react';
import axios from 'axios';

const API_KEY = '274f95ad3cfc436484e13611251207';

function Weather() {
  const city = localStorage.getItem('weatherCity');
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    if (city) {
      axios
        .get(`https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${API_KEY}&units=metric`)
        .then((res) => setData(res.data))
        .catch(console.error)
        .finally(() => setLoading(false));
    }
  }, [city]);

  const temperatureF = useMemo(() => {
    return data ? (data.main.temp * 9) / 5 + 32 : null;
  }, [data]);

  if (loading) return <p>Loading...</p>;
  if (!data) return <p>No weather data available.</p>;

  return (
    <div className="weather-card">
      <h2>Weather in {data.name}</h2>
      <p>🌡️ Temperature: {data.main.temp}°C / {temperatureF.toFixed(2)}°F</p>
      <p>💧 Humidity: {data.main.humidity}%</p>
      <p>💨 Wind Speed: {data.wind.speed} m/s</p>
      <p>☁️ Condition: {data.weather[0].description}</p>
    </div>
  );
}

export default Weather;
```
App.jsx
```
import { useState } from 'react'
import { Routes, Route, Link } from 'react-router-dom';
import './App.css'
import Weather from './Weather'
import Home from './Home'

function App() {
  return (
    <div className="container">
      <nav className="nav">
        <Link to="/">Home</Link>
        <Link to="/weather">Weather</Link>
      </nav>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/weather" element={<Weather />} />
      </Routes>
    </div>
  )
}

export default App
```

Home.jsx
```
import { useState, useCallback } from 'react';
import { useNavigate } from 'react-router-dom';

function Home() {
  const [city, setCity] = useState('');
  const [error, setError] = useState('');
  const navigate = useNavigate();

  const handleSubmit = useCallback((e) => {
    e.preventDefault();
    if (!city.trim()) {
      setError('Please enter a city name');
    } else {
      localStorage.setItem('weatherCity', city);
      navigate('/weather');
    }
  }, [city, navigate]);

  return (
    <div className="form-container">
      <h1>Enter City for Weather</h1>
      <form onSubmit={handleSubmit}>
        <input
          type="text"
          value={city}
          onChange={(e) => { setCity(e.target.value); setError(''); }}
          placeholder="Enter city"
        />
        <button type="submit">Get Weather</button>
        {error && <p className="error">{error}</p>}
      </form>
    </div>
  );
}

export default Home;
```

.env
```
API_KEY = '274f95ad3cfc436484e13611251207'
```
## Output:
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e376f6f9-34af-4e80-bd10-ac1715a5fdd8" />

## Result:
A responsive single-page application using React that allows users to enter a city name and retrieve real-time weather information using the OpenWeatherMap API has been built successfully. 
