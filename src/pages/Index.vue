<template>
  <q-page class="flex column" :class="backgroundClass">
    <div class="col q-pt-lg q-px-md">
      <q-input 
        v-model="search"
        @keyup.enter="getWeatherBySearch" 
        placeholder="Search"  
        dark
        borderless
      >
        <template v-slot:before>
          <q-icon 
            @click="getLocation"
            name="location_on"
          />
        </template>

        <template v-slot:append>
          <q-btn 
            @click="getWeatherBySearch"
            round 
            dense 
            flat 
            icon="search" 
          />
        </template>
      </q-input>
    </div>

    <template v-if="weatherData">
      <div class="col text-white text-center">
        <div class="text-h4 text-weight-light">
          {{ weatherData.name }}
        </div>
        <div class="text-h6 text-weight-light">
          {{ weatherData.weather[0].main }}
        </div>
        <div class="text-h1 text-weight-thin q-my-lg relative-position">
          <span>{{ Math.round(weatherData.weather[0].main) }}</span>
          <span class="degree text-h4 relative-position">&deg;C</span>
        </div>
      </div>

      <div class="col text-center">
        <img :src="`https://openweathermap.org/img/wn/${ weatherData.weather[0].icon}@2x.png`" alt="Image">
      </div>
    </template>
    
    <template v-else>
      <div class="col column text-center text-white">
        <div class="col text-h2 text-weight-thin">
          Drizzy <br> Weather
        </div>
        <q-btn 
          @click="getLocation"
          class="col" 
          flat
        >
          <q-icon left size="3em" name="travel_explore" />
          <div>Find my location</div>
        </q-btn>
      </div>
    </template>

    <div class="col skyline">
    </div>
  </q-page>
</template>

<script>
export default {
  name: 'PageIndex',
  data() {
    return {
      search: '',
      weatherData: null,
      lat: null,
      lon: null,
      apiUrl: 'https://api.openweathermap.org/data/2.5/weather',
      apiKey: '18ca215a560311b1a7a444eba3a60283'
    }
  },
  computed: {
    backgroundClass() {
      if (this.weatherData) {
        if (this.weatherData.weather[0].icon.endsWith('n')) {
          return 'bg-night'
        } else {
          return 'bg-day'
        }
      }
    }
  },
  methods: {
    getLocation() {
      this.$q.loading.show()

      if (this.$q.platform.is.electron) {
        this.$axios.get('https://freegeoip.app/json/')
        .then(response => {
          this.lat = response.data.latitude
          this.lon = response.data.longitude
          this.getWeatherByCoords()
        })
      } 
      
      else {
        navigator.geolocation.getCurrentPosition
        (position => {
          console.log('position: ', position)
          this.lat = position.coords.latitude
          this.lon = position.coords.longitude
          this.getWeatherByCoords()
        })
      } 
    },
    getWeatherByCoords() {
      this.$q.loading.show()
      this.$axios(`${ this.apiUrl }?lat=${ this.lat }
      &lon=${ this.lon }&appid=${ this.apiKey }
      &units=metric`).then(response => {
        this.weatherData = response.data
        this.$q.loading.hide()
      })
    },
    getWeatherBySearch() {
      this.$q.loading.show()
      this.$axios(`${ this.apiUrl }?q=${ this.search }&appid=${ this.apiKey }
      &units=metric`).then(response => {
        this.weatherData = response.data
        this.$q.loading.hide()
      })
    }
  }
}
</script>

<style lang="sass">
  .q-page
    background: linear-gradient(to bottom, #fc4a1a, #f7b733)
    &.bg-day
      background: linear-gradient(to right, #2980b9, #6dd5fa, #ffffff)
    &.bg-night
      background: linear-gradient(to right, #232526, #414345)
  .degree
    top: -40px
  .skyline 
    flex: 0 0 100px
    background: url(../statics/skyline.png)
    background-size: contain
    background-position: center bottom
</style>
