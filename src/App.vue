<template>
  <div id="app" :class="weather.main && weather.main.temp > 16 ? 'warm' : ''">
    <div class="container">
      <!-- search bar -->
      <div class="search-box">
        <input type="text" placeholder="Search...." class="search-bar" v-model="query" @keyup.enter="fetchWeather"/>
      </div>

      <div class="weather-wrapper">
        <!-- location & date info -->
        <div class="location-box">
          <div class="location">{{weather.name}}</div>
          <div class="date">{{currentDate}}</div>
        </div>

        <!-- weather info -->
        <div class="weather-box">
          <div class="temperature">{{ Math.round(weather.main.temp)  }}°C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </div>
  </div>
</template>


<script >
  import dayjs from 'dayjs'
  import advancedFormat from 'dayjs/plugin/advancedFormat'
  dayjs.extend(advancedFormat)

  export default {
  name: 'App',
  data() {
    return {
      api_key: import.meta.env.VITE_WEATEHR_API_KEY,
      base_url: 'https://api.openweathermap.org/data/2.5/',
      query: 'Taichung',
      weather: {},
      date: ''
    }
  },
  methods: {
    async fetchWeather() {
      const data = await fetch(`${this.base_url}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
      this.weather = await data.json()
      console.log(this.weather)
      console.log("data",data)
    }
  },
  created() {
    this.fetchWeather()
  },
  computed: {
    currentDate() {
      return dayjs().format(`MMMM Do YYYY`)
    }
  }
}
</script>

<style></style>


