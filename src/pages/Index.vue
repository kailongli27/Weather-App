<template>
  <q-page class="column bg-grey-4">
    <div class="column text-center bg-orange-13">
      <div class="self-center text-h3 text-weight-light text-white q-mt-lg">
        Weather Check
      </div>
      <q-input rounded standout="bg-orange-5" v-model="city" @keyup.enter="getWeatherData()" placeholder="Enter a city name" dense input-class="text-black" class="q-mt-lg q-mb-sm self-center text-body1" style="width: 35%">
        <template v-slot:prepend>
          <q-icon name="place" class="text-black"></q-icon>
        </template>
        <template v-slot:append>
          <q-icon name="search" @click="getWeatherData()" class="cursor-pointer text-black"></q-icon>
        </template>
      </q-input>
      <q-btn-toggle
        v-model="unitSelection"
        class="self-center q-mt-md q-mb-md"
        rounded
        ripple
        unelevated
        toggle-color="blue-grey-5"
        color="orange-12"
        text-color="white"
        toggle-text-color="white"
        :options="[
          {label: 'C', value: 'metric'},
          {label: 'F', value: 'imperial'}
        ]"
      />
    </div>
    <div v-if="unitSelection === 'metric'" class="self-center q-mt-xl">
      <q-card class="rounded q-pa-lg q-mt-lg text-center">
        <div class="text-h4 q-mb-sm">{{ this.queryCity.name }}, {{this.queryCity.country}}</div>
        <div class="text-h5 text-orange-10">{{ this.queryCity.temp }}</div>
        <div class="text-italic text-no-wrap q-mb-md">Feels like {{ this.queryCity.feelsLike }}</div>
        <div>{{ this.queryCity.description }}</div>
        <div><q-img :src="this.queryCity.iconURL" alt="Weather icon"/></div>
      </q-card>
    </div>
  </q-page>
</template>

<script>
import axios from 'axios'

export default {
  name: 'WeatherPage',
  data () {
    return {
      city: '',
      unitSelection: 'metric',
      weatherAPI: {
        key: 'ad0ddaff0572e83a6fcf96cf7a2e3ade'
      },
      queryCity: {
        name: null,
        country: null,
        temp: null,
        description: null,
        feelsLike: null,
        iconURL: null
      }
    }
  },
  methods: {
    getWeatherData () {
      console.log(this.city)
      axios.get(`http://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=${this.unitSelection}&appid=${this.weatherAPI.key}`)
        .then((res) => {
          console.log(res)
          this.queryCity.name = res.data.name
          this.queryCity.country = res.data.sys.country
          this.queryCity.temp = res.data.main.temp
          this.queryCity.description = res.data.weather[0].description.charAt(0).toUpperCase() + res.data.weather[0].description.slice(1)
          this.queryCity.feelsLike = res.data.main.feels_like
          this.queryCity.iconURL = `http://openweathermap.org/img/wn/${res.data.weather[0].icon}@2x.png`
          console.log(this.queryCity.iconURL)
        })
        .catch(() => {
          alert('City not found!')
          console.log('City not found!')
        })
    }
  }
}
</script>
