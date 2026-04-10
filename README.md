<div align="center">
  <h1>🌦️ Nebula Weather App (Android)</h1>
  <p>A beautifully animated, highly interactive Android weather application built entirely with <b>Kotlin</b> and <b>Jetpack Compose</b>.</p>

  <p>
    <a href="#-features">Features</a> •
    <a href="#-architecture">Architecture</a> •
    <a href="#-project-structure">Project Structure</a> •
    <a href="#-api--workflow">API & Workflow</a> •
    <a href="#-installation">Installation</a>
  </p>
</div>

---

## ✨ Features

- **Real-Time Forecasting:** Granular, up-to-date weather metrics including temperature, humidity, wind conditions, and multi-day forecasting.
- **Glassmorphic UI Engine:** A sleek, premium, dark-themed UI relying heavily on translucency, subtle blurs, and contrasting borders to achieve modern glassmorphism.
- **Predictive Location Search:** Integrated auto-suggest logic allowing seamless lookups of global cities with fast predictive caching.
- **Dynamic Day/Night Transitions:** Automatically calculates local sunrise and sunset times to transition the application's backdrop from day to night accurately.
- **Micro-Animations & Canvas Rendering:** Fully custom particle systems and canvas-drawn vector animations spanning across states (e.g. 🌧️ Rain drops, ☀️ Sun glare, 🌩️ Thunderstorms, etc.).
- **Auto-Refresh Loop:** Synchronizes silently in the background every 5 minutes to ensure displayed data is never stale.

---

## 🏗 Architecture

This project is structured using modern **MVVM (Model-View-ViewModel)** best practices along with **Clean Architecture** concepts to rigidly separate UI layers from the business and data logic.

- **UI Layer (Jetpack Compose):** Utterly decoupled from logic. The views simply subscribe to Kotlin `StateFlow` streams exposed by the ViewModel and reactively rebuild when the state (`WeatherUiState`) changes.
- **Presentation Logic (ViewModel):** Handles the timer for the auto-refresh loop, dispatches Coroutines to fetch network data, processes raw data into structured sealed states, and triggers UI updates.
- **Data Layer (Repository):** Abstracts the origin of weather data, funneling the REST API calls and converting network-level objects into domain-ready models.
- **Network Layer (Retrofit & OkHttp):** Constructs safe, typed endpoints mapped directly to OpenWeather API JSON payloads using Gson.

---

## 📂 Project Structure

```text
app/src/main/java/com/weatherapp/
├── api/                  # Retrofit Clients and API Interfaces
├── components/           # Reusable Jetpack Compose UI functions (Buttons, GlassCards, Backgrounds)
├── drawing/              # Low-level Canvas API functions for generating weather effects & particles
├── model/                # Data Transfer Objects (WeatherResponse, ForecastResponse, LocationResult)
├── repository/           # Data Abstraction Layer & network dispatchers
├── state/                # Kotlin Sealed Classes (Sunny, Cloudy, Thunderstorm, Night, Rain, Fog, Snow)
├── viewmodel/            # WeatherViewModel containing business logic and StateFlow emissions
└── MainActivity.kt       # Application entry point and top-level NavHost / Compose configuration
```

---

## ⚙️ API & Workflow

### The OpenWeather API
The core of the application retrieves its forecasting logic securely from the **[OpenWeatherMap API](https://openweathermap.org/api)**. 
- **Geocoding API:** Used to parse typed city names in the predictive search bar into accurate Latitude (Lat) and Longitude (Lon) coordinates.
- **Current Weather API:** Fetches the specific, real-time snapshot of the weather state, temperature, and sun positioning.
- **5-Day Forecast API:** Supplements the bottom UI scrollable sequence with multi-day projections.

### Data Workflow
1. **User Input:** The user types a city name.
2. **Geocoding:** `WeatherViewModel` dispatches a Coroutine to the `WeatherRepository` to convert the string location into `(Lat, Lon)`.
3. **Data Fetching:** The Repository asynchronously requests OpenWeather for the current state and forecast arrays using Retrofit.
4. **State Calculation:** The JSON response is mapped into memory models (`WeatherResponse.java`). The ViewModel calculates whether the current time lies between the `sunrise` and `sunset` UNIX timestamps to emit either a Day or Night base theme.
5. **UI Rendering:** Depending on the `WeatherState` sealed class (e.g., `WeatherState.Rain`), Jetpack Compose triggers the specific animated Canvas functions located in `AnimatedWeatherBackground.kt` and renders the Glassmorphic data cards.
6. **Poller Loop:** A repeating background thread sleeps for 5 minutes, validates the last location, and loops back to step 3.

---

## 📦 Installation & Setup

### Prerequisites
- [Android Studio Iguana](https://developer.android.com/studio) (or newer)
- Kotlin Version > `1.9.0`
- Minimum SDK: `API 26` (Android 8.0 Oreo)

### Building the Project
1. **Clone the repository:**
   ```bash
   git clone https://github.com/harshalthul/weather-app-.git
   ```
2. **Open the project** inside Android Studio.
3. **Configuring the API Key:**
   - Sign up for a free API key at [OpenWeatherMap](https://home.openweathermap.org/users/sign_up).
   - *If applicable, add your `API_KEY` into `local.properties` (or inject it directly into the `WeatherApiService` interface depending on your flavor).*
4. **Resync Gradle** to pull Jetpack Compose and Retrofit dependencies.
5. **Run the App:** Click the green **Run (▶)** button while an Emulator or real device is selected!

> *Note: If you only want to install the fully built testing application, you can download the built APK directly from the **[APK Repository](https://github.com/harshthul90-eng/weather-app-apk)**.*

---
*Built via Modern Android Technologies.*