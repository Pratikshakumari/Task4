# Task4
# HTML Part
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>

  <title>Weather Dashboard</title>

  <!-- CSS FILE -->
  <link rel="stylesheet" href="style.css">
</head>

<body>

  <div class="weather-container">

    <h1>Weather Dashboard</h1>

    <!-- SEARCH SECTION -->
    <div class="search-box">

      <input
        type="text"
        id="cityInput"
        placeholder="Enter city name"
      />

      <button id="searchBtn">
        Search
      </button>

    </div>

    <!-- ERROR MESSAGE -->
    <p id="errorMessage"></p>

    <!-- WEATHER CARD -->
    <div class="weather-card" id="weatherCard">

      <h2 id="cityName">City Name</h2>

      <p>
        🌡 Temperature:
        <span id="temperature">--</span> °C
      </p>

      <p>
        💧 Humidity:
        <span id="humidity">--</span> %
      </p>

      <p>
        🌬 Wind Speed:
        <span id="windSpeed">--</span> km/h
      </p>

      <p>
        🌤 Condition:
        <span id="condition">--</span>
      </p>

    </div>

  </div>

  <!-- JS FILE -->
  <script src="script.js"></script>

</body>
</html>


# CSS Part

/* =========================
   GLOBAL STYLES
========================= */

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: Arial, sans-serif;
  background: linear-gradient(to right, #2563eb, #60a5fa);
  min-height: 100vh;

  display: flex;
  justify-content: center;
  align-items: center;

  padding: 20px;
}

/* =========================
   WEATHER CONTAINER
========================= */

.weather-container {
  width: 100%;
  max-width: 450px;
  background: white;
  padding: 30px;
  border-radius: 16px;
  text-align: center;

  box-shadow: 0 4px 15px rgba(0,0,0,0.2);
}

h1 {
  margin-bottom: 20px;
  color: #2563eb;
}

/* =========================
   SEARCH BOX
========================= */

.search-box {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.search-box input {
  flex: 1;
  padding: 12px;
  border: 1px solid #ccc;
  border-radius: 8px;
  font-size: 1rem;
}

.search-box button {
  padding: 12px 18px;
  border: none;
  background: #2563eb;
  color: white;
  border-radius: 8px;
  cursor: pointer;
}

.search-box button:hover {
  background: #1d4ed8;
}

/* =========================
   WEATHER CARD
========================= */

.weather-card {
  background: #f4f4f4;
  padding: 20px;
  border-radius: 12px;
}

.weather-card h2 {
  margin-bottom: 15px;
  color: #1e3a8a;
}

.weather-card p {
  margin: 10px 0;
  font-size: 1.1rem;
}

/* =========================
   ERROR MESSAGE
========================= */

#errorMessage {
  color: red;
  margin-bottom: 15px;
  font-weight: bold;
}

/* =========================
   RESPONSIVE DESIGN
========================= */

@media (max-width: 500px) {

  .search-box {
    flex-direction: column;
  }

  .search-box button {
    width: 100%;
  }
}


# JS Part
/* =========================
   API CONFIGURATION
========================= */

/*
  Get FREE API Key from:
  https://openweathermap.org/api
*/

const API_KEY = "YOUR_API_KEY";

/* =========================
   DOM ELEMENTS
========================= */

const cityInput = document.getElementById("cityInput");
const searchBtn = document.getElementById("searchBtn");

const cityName = document.getElementById("cityName");
const temperature = document.getElementById("temperature");
const humidity = document.getElementById("humidity");
const windSpeed = document.getElementById("windSpeed");
const condition = document.getElementById("condition");

const errorMessage = document.getElementById("errorMessage");

/* =========================
   FETCH WEATHER DATA
========================= */

async function getWeather(city) {

  try {

    errorMessage.textContent = "";

    const url =
      `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${API_KEY}&units=metric`;

    const response = await fetch(url);

    /* ERROR HANDLING */

    if (!response.ok) {
      throw new Error("City not found");
    }

    /* PARSE JSON */

    const data = await response.json();

    console.log(data);

    /* EXTRACT DATA */

    const cityData = data.name;

    const tempData = data.main.temp;

    const humidityData = data.main.humidity;

    const windData = data.wind.speed;

    const conditionData = data.weather[0].main;

    /* RENDER DATA */

    cityName.textContent = cityData;

    temperature.textContent = tempData;

    humidity.textContent = humidityData;

    windSpeed.textContent = windData;

    condition.textContent = conditionData;

  }

  catch (error) {

    errorMessage.textContent = error.message;

    cityName.textContent = "City Name";

    temperature.textContent = "--";

    humidity.textContent = "--";

    windSpeed.textContent = "--";

    condition.textContent = "--";

  }

}

/* =========================
   SEARCH BUTTON EVENT
========================= */

searchBtn.addEventListener("click", () => {

  const city = cityInput.value.trim();

  if (city === "") {

    errorMessage.textContent = "Please enter a city name";

    return;
  }

  getWeather(city);

});

/* =========================
   ENTER KEY SUPPORT
========================= */

cityInput.addEventListener("keypress", (e) => {

  if (e.key === "Enter") {

    searchBtn.click();

  }

});
