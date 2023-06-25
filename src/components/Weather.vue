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
    </div>
  </div>
</template>

<script>
import { defineComponent } from 'vue';
import axios from 'axios';
const OPENWEATHERMAP_URL = import.meta.env.VITE_OPENWEATHERMAP_URL;
const OPENWEATHERMAP_API = import.meta.env.VITE_OPENWEATHERMAP_API;
const OPENWEATHERMAP_APPID = import.meta.env.VITE_OPENWEATHERMAP_APPID;

export default defineComponent({
  name: 'Weather',
  data() {
    return {
      cityName: 'Toulouse',
    }
  },
  created() {
    this.getWeather();
    this.getWeatherIcon();
  },
  computed: {
  },
  methods: {
    async getWeather() {
      const data = await axios.get(`${OPENWEATHERMAP_API}weather?q=${this.cityName}&units=metric&APPID=${OPENWEATHERMAP_APPID}`)
        .then((response) => {
          console.log(response.data);
          return response.data;
        })
        .catch(() => {});
      return data;
    },
    getWeatherIcon(iconCode = '10d') {
      return axios.get(`${OPENWEATHERMAP_URL}img/w/${iconCode}.png`)
        .then((response) => response.data)
        .catch(() => {});
    },
  },
});

</script>
<style scoped>
.read-the-docs {
  color: #888;
}
</style>
