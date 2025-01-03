<template>
  <div>
    <input
      v-model="query"
      type="text"
      placeholder="Search for a location"
      @input="debounceSearch"
    />
    <ul v-if="locations.length > 0">
      <li
        v-for="location in locations"
        :key="location.id"
        @click="selectLocation(location)"
      >
        {{ location.name }}, {{ location.country }}
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      query: "",
      locations: [],
      debounceTimeout: null,
    };
  },
  methods: {
    async searchLocations() {
      if (this.query.trim() === "") return;

      const url = `https://geocoding-api.open-meteo.com/v1/search?name=${encodeURIComponent(
        this.query
      )}&count=10&language=en&format=json`;

      try {
        const response = await axios.get(url);
        this.locations = response.data.results || [];
      } catch (error) {
        console.error("Error fetching locations:", error);
      }
    },
    debounceSearch() {
      clearTimeout(this.debounceTimeout);
      this.debounceTimeout = setTimeout(() => this.searchLocations(), 500);
    },
    selectLocation(location) {
      this.$emit("locationSelected", location);
      this.query = "";
      this.locations = [];
    },
  },
};
</script>

<style scoped>
ul {
  list-style: none;
  padding: 0;
}
li {
  cursor: pointer;
  padding: 5px;
}
li:hover {
  background-color: #f0f0f0;
}
</style>
