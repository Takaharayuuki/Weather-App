<template>
  <q-page class="flex column">
    <div class="col q-pt-lg q-px-md">
     <q-input
        dark
        v-model="search"
        label="Search"
        borderless >
        <template v-slot:before>
          <q-icon name="my_location"
                  @click="getLocation" />
        </template>

        <template v-slot:hint>
          Field hint
        </template>

        <template v-slot:append>
         <q-btn
            round
            dense
            flat
            icon="add" />
        </template>
      </q-input>
    </div>

    <template v-if="weatherData">
      <div class="col text-white text-center">
        <div class="text-h4 text-weight-light">
          Saitama
        </div>
        <div class="text-h6 text-weight-light">
          Sunny
        </div>
        <div class="text-h1 text-weight-thin q-my-lg relative-position">
          <span>8</span>
          <span class="text-h4 relative-position degree">&deg;</span>
        </div>
      </div>

      <div class="col text-center">
        <img src="https://www.fillmurray.com/100/100" alt="Bill">
      </div>
    </template>

    <template v-else>
      <div class="col column text-center text-white">
        <div class="col text-h2 text-weight-thin">
          Weather<br>App
        </div>
        <q-btn
          @click="getLocation"
          class="col"
          flat>
          <q-icon
            left
            size="3em"
            name="my_location" />
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
      search:'',
      weatherData: null,
      lat: null,
      lon: null,
      apiUrl: 'http://api.openweathermap.org/data/2.5/weather',
      apiKey: ''
    }
  },
  methods: {
    getLocation() {
      navigator.geolocation.getCurrentPosition(position => {
        console.log(position)
        this.lat = position.coords.latitude
        this.lon = position.coords.longitude
        this.getWeatherByCoods()
      })
    },
    getWeatherByCoods() {
      this.$axios(`${ this.apiUrl }?lat=${ this.lat }&lon=${ this.lon }&appid=${ this.apiKey }&units=metric`).then(response => {
        console.log(response);
      })
    }
  }
}
</script>

<style lang="scss" scoped>
  .q-page {
    background: linear-gradient(to bottom, #0b486b, #f56217);
  }
  .degree {
    top: -44px;
  }
  .skyline {
    flex: 0 0 100px;
    background: url(../assets/city2.png);
    background-size: contain;
    background-position:  center bottom;
  }
</style>
