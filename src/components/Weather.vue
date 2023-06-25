<script setup>
import { ref } from 'vue'

defineProps({
  msg: String,
})

// const OPENWEATHERMAP_URL = ref(import.meta.env.VITE_OPENWEATHERMAP_URL);
// const OPENWEATHERMAP_API = ref(import.meta.env.VITE_OPENWEATHERMAP_API);
// const OPENWEATHERMAP_APPID = ref(import.meta.env.VITE_OPENWEATHERMAP_APPID);
</script>

<template>
  <h1>{{ msg }}</h1>

  <div class="container">
    <div class="content">
      <v-text-field
        ref="weatherInput"
        color="primary"
        shaped
        outlined
        v-model="cityName"
        class="custom-label-color"
        placeholder="Enter your city"
        @keyup.enter="getWeather()"
        >
      </v-text-field>
      <v-btn
        block
        rounded
        color="success"
        :disabled="disabledSearchButton"
        @click="getWeather()"
      >Search</v-btn>
      <v-snackbar
        v-model="showAlert"
        timeout="2000"
      >
        Please try another city.
      <template v-slot:actions>
          <v-btn
            color="blue"
            variant="text"
            @click="alart = false"
          >
            Close
          </v-btn>
        </template>
      </v-snackbar>
      <WeatherInfo
        v-if="!showAlert && weatherDetails"
        :weatherDetails="weatherDetails"
      />
    </div>
  </div>
</template>

<script>
import { defineComponent } from 'vue';
import axios from 'axios';
const OPENWEATHERMAP_URL = import.meta.env.VITE_OPENWEATHERMAP_URL;
const OPENWEATHERMAP_API = import.meta.env.VITE_OPENWEATHERMAP_API;
const OPENWEATHERMAP_APPID = import.meta.env.VITE_OPENWEATHERMAP_APPID;

import WeatherInfo from './WeatherInfo.vue'

export default defineComponent({
  name: 'Weather',
  components: [WeatherInfo],
  data() {
    return {
      cityName: 'Paris',
      weatherDetails: null,
      showAlert: false,
    }
  },
  created() {
    this.getWeather();
  },
  computed: {
    disabledSearchButton() {
      return !this.cityName || this.cityName.length < 2;
    },
  },
  methods: {
    async getWeather() {
      this.showAlert = false;
      const data = await axios.get(`${OPENWEATHERMAP_API}weather?q=${this.cityName}&units=metric&APPID=${OPENWEATHERMAP_APPID}`)
        .then((response) => response.data)
        .then((res) => this.setWeatherDetails(res))
        .catch((e) => this.displayError(e));
      return data;
    },
    setWeatherDetails(details) {
      const iconCode = details.weather && details.weather.length > 0 ? details.weather[0].icon : '01d';
      this.weatherDetails = {
        title: details.name,
        temp: details.main.temp.toFixed(1),
        humidity: details.main.humidity,
        icon: `${OPENWEATHERMAP_URL}img/w/${iconCode}.png`,
        wind: details.wind.speed,
      };
      this.cityName = null;
    },
    displayError() {
      this.showAlert = true;
      // reset cityName
      this.cityName = null;
      // reset weatherDetails
      this.weatherDetails = null;
    },
  },
});

</script>
<style scoped>
.read-the-docs {
  color: #888;
}
</style>
