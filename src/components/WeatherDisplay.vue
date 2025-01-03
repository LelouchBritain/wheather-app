<template>
  <div>
    <h2>Weather Forecast</h2>
    <p v-if="data.current_weather">
      Current Temperature: {{ data.current_weather.temperature }}°C
    </p>
    <ul>
      <li v-for="(temp, index) in hourlyTemperatures" :key="index">
        {{ formatLocalTime(index) }}: {{ temp }}°C
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: ["data"],
  computed: {
    hourlyTemperatures() {
      return this.data.hourly.temperature_2m || [];
    },
  },
  methods: {
    formatLocalTime(index) {
      const timestamp = this.data.hourly.time[index];
      const date = new Date(timestamp);
      return date.toLocaleString([], { hour: "2-digit", minute: "2-digit" });
    },
  },
};
</script>

<style scoped>
ul {
  list-style: none;
  padding: 0;
}
</style>
