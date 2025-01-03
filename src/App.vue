<template>
  <div class="app">
    <h1>Weather Search App</h1>
    <LocationSearch @locationSelected="fetchWeather" />
    <WeatherDisplay v-if="weatherData" :data="weatherData" />
  </div>
</template>

<script>
import LocationSearch from "./components/LocationSearch.vue";
import WeatherDisplay from "./components/WeatherDisplay.vue";
import axios from "axios";

export default {
  components: { LocationSearch, WeatherDisplay },
  data() {
    return {
      weatherData: null,
    };
  },
  methods: {
    async fetchWeather(location) {
      const { latitude, longitude } = location;

      const startDate = new Date().toISOString().split("T")[0];
      const endDate = new Date(Date.now() + 86400000)
        .toISOString()
        .split("T")[0];

      const url = `https://api.open-meteo.com/v1/forecast?latitude=${latitude}&longitude=${longitude}&hourly=temperature_2m&start_date=${startDate}&end_date=${endDate}&timezone=auto`;

      try {
        const response = await axios.get(url);
        this.weatherData = response.data;
      } catch (error) {
        console.error("Error fetching weather data:", error);
      }
    },
  },
};
</script>

<style>
.app {
  text-align: center;
  padding: 20px;
}
</style>
