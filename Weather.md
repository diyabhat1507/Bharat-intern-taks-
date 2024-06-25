<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>Weather App</title>
    <link rel="stylesheet" href="style.css">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <script src="script.js" defer></script>
  </head>
  <body>
    <h1>Weather Dashboard</h1>
    <div class="container">
      <div class="weather-input">
        <h3>Enter a City Name</h3>
        <input class="city-input" type="text" placeholder="E.g., New York, London, Tokyo">
        <button class="search-btn">Search</button>
        <div class="separator"></div>
        <button class="location-btn">Use Current Location</button>
      </div>
      <div class="weather-data">
        <div class="current-weather">
          <div class="details">
            <h2 id="city-name">_______ ( ______ )</h2>
            <h6 id="temperature">Temperature: __Â°C</h6>
            <h6 id="wind">Wind: __ M/S</h6>
            <h6 id="humidity">Humidity: __%</h6>
          </div>
        </div>
        <div class="days-forecast">
          <h2>5-Day Forecast</h2>
          <ul class="weather-cards" id="forecast-list">
            <!-- forecast cards will be generated here -->
          </ul>
        </div>
      </div>
    </div>
  </body>
</html>

body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
}

.container {
  max-width: 800px;
  margin: 40px auto;
  padding: 20px;
  background-color: #f9f9f9;
  border: 1px solid #ddd;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.weather-input {
  margin-bottom: 20px;
}

.city-input {
  width: 100%;
  padding: 10px;
  font-size: 16px;
  border: 1px solid #ccc;
}

.search-btn, .location-btn {
  padding: 10px 20px;
  font-size: 16px;
  border: none;
  border-radius: 5px;
  background-color: #4CAF50;
  color: #fff;
  cursor: pointer;
}

.search-btn:hover, .location-btn:hover {
  background-color: #3e8e41;
}

.separator {
  margin: 20px 0;
  border-bottom: 1px solid #ccc;
}

.weather-data {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}

.current-weather {
  flex-basis: 40%;
  margin-right: 20px;
}

.days-forecast {
  flex-basis: 60%;
}

.weather-cards {
  list-style: none;
  padding: 0;
  margin: 0;
}

.weather-cards li {
  background-color: #f9f9f9;
  padding: 10px;
  border: 1px solid #ddd;
  margin-bottom: 10px;
}

.weather-cards li h3 {
  margin-top: 0;
}

const apiKey = 'YOUR_OPENWEATHERMAP_API_KEY';
const apiUrl = 'https://api.openweathermap.org/data/2.5';

const cityInput = document.querySelector('.city-input');
const searchBtn = document.querySelector('.search-btn');
const locationBtn = document.querySelector('.location-btn');
const cityNameElement = document.querySelector('#city-name');
const temperatureElement = document.querySelector('#temperature');
const windElement = document.querySelector('#wind');
const humidityElement = document.querySelector('#humidity');
const forecastList = document.querySelector('#forecast-list');

searchBtn.addEventListener('click', searchCity);
locationBtn.addEventListener('click', getLocation);

function searchCity() {
  const city = cityInput.value.trim();
  if (city) {
    fetchWeatherData(city);
  }
}

function getLocation() {
  if (navigator.geolocation) {
    navigator.geolocation.getCurrentPosition(position => {
      const lat = position.coords.latitude;
      const lon = position.coords.longitude;
      fetchWeatherData(lat, lon);
    });
  }
}

function fetchWeatherData(city, lat, lon) {
  let url = `${apiUrl}/weather
