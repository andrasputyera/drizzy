<template>
  <q-page class="flex column">
    <div class="col q-pt-lg q-px-md">
      <q-input 
        v-model="search" 
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
          <q-btn round dense flat icon="search" />
        </template>
      </q-input>
    </div>

    <template v-if="weatherData">
      <div class="col text-white text-center">
        <div class="text-h4 text-weight-light">
          Sydney
        </div>
        <div class="text-h6 text-weight-light">
          Sunny
        </div>
        <div class="text-h1 text-weight-thin q-my-lg relative-position">
          <span>8</span>
          <span class="degree text-h4 relative-position">&deg;</span>
        </div>
      </div>

      <div class="col text-center">
        <img src="https://via.placeholder.com/100/100" alt="Image">
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
      latitude: null,
      longitude: null,
      apiUrl: 'http://api.openweathermap.org/data/2.5/weather',
      apiKey: '18ca215a560311b1a7a444eba3a60283'
    }
  },
  methods: {
    getLocation() {
      navigator.geolocation.getCurrentPosition
      (position => {
        console.log('position: ', position)
        this.latitude = position.coords.latitude
        this.longitude = position.coords.longitude
        this.getWeatherByCoords()
      })
    },
    getWeatherByCoords() {
      this.$axios(`${ this.apiUrl }?lat=${ this.latitude }
      &lon=${ this.longitude }&appid=${ this.apiKey }
      &units=metric`).then(response => {
        console.log('response: ', response)
      })
    }
  }
}
</script>

<style lang="sass">
  .q-page
    background: linear-gradient(to bottom, #fc4a1a, #f7b733)
  .degree
    top: -40px
  .skyline 
    flex: 0 0 100px
    background: url(../statics/skyline.png)
    background-size: contain
    background-position: center bottom
</style>
