<template>
  <q-page class="flex column" :class="bgClass">
    <div class="col q-pt-lg q-px-md">
     <q-input
        dark
        @keyup.enter="getWeatherBySearch"
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
            @click="getWeatherBySearch"
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
          {{ weatherData.name }}
        </div>
        <div class="text-h6 text-weight-light">
          {{ weatherData.weather[0].main }}
        </div>
        <div class="text-h1 text-weight-thin q-my-lg relative-position">
          <span>{{ Math.round(weatherData.main.temp) }}</span>
          <span class="text-h4 relative-position degree">&deg;C</span>
        </div>
      </div>

      <div class="col text-center">
        <img :src="`http://openweathermap.org/img/wn/${ weatherData.weather[0].icon }@2x.png`">
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
      apiKey: '35d6476cda4fd20be5023b1c2b4d0b3b',
    }
  },
  methods: {
    getLocation() {
      this.$q.loading.show()

      //? electron(Mac対応)だったら
      if(this.$q.platform.is.electron) {
        this.$axios.get('https://freegeoip.app/json/').then(response => {
          this.lat = response.data.latitude
          this.lon = response.data.longitude
          this.getWeatherByCoods()
        })
      }

      else {
        navigator.geolocation.getCurrentPosition(position => {
          console.log(position)
          this.lat = position.coords.latitude
          this.lon = position.coords.longitude
          this.getWeatherByCoods()
        })
      }
    },
    getWeatherByCoods() {
      this.$q.loading.show()
      this.$axios(`${ this.apiUrl }?lat=${ this.lat }&lon=${ this.lon }&appid=${ this.apiKey }&units=metric`).then(response => {
        console.log(response);
        this.weatherData = response.data
      })
      this.$q.loading.hide()
    },
    getWeatherBySearch() {
      this.$q.loading.show()
      console.log('getWeatherBysearch');
      this.$axios(`${ this.apiUrl }?q=${ this.search }&appid=${ this.apiKey }&units=metric`).then(response => {
        console.log(response);
        this.weatherData = response.data
      })
      this.$q.loading.hide()
    }
  },
  computed: {
    bgClass() {
      if(this.weatherData) {
        if(this.weatherData.weather[0].icon.endsWith('n')) {
          return 'bg-nihgt'
        }
        else {
          return 'bg-day'
        }
      }
    }
  }
}
</script>

<style lang="scss" scoped>
  .q-page {
    background: linear-gradient(to bottom, #0b486b, #f56217);
    &.bg-night {
      background: #232526;  /* fallback for old browsers */
      background: -webkit-linear-gradient(to right, #414345, #232526);  /* Chrome 10-25, Safari 5.1-6 */
      background: linear-gradient(to right, #414345, #232526); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
    }
    &.bg-day {
      background: #00b4db; /* fallback for old browsers */
      background: -webkit-linear-gradient(to right, #00b4db, #0083b0); /* Chrome 10-25, Safari 5.1-6 */
      background: linear-gradient(to right, #00b4db, #0083b0); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
    }
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
