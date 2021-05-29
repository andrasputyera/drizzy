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
          <q-icon name="location_on" />
        </template>

        <template v-slot:append>
          <q-btn round dense flat icon="search" />
        </template>
      </q-input>
    </div>

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
      weatherData: null
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
