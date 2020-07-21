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
        @click="unitsChange()"
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
    <div v-if="queryEntered && !queryError" class="self-center q-mt-xl">
      <q-card class="rounded q-pa-sm q-mt-lg" style="width: 500px">
        <q-card-section class="column col">
          <div class="row no-wrap items-center">
            <div class="text-h4 text-no-wrap">{{ this.queryCity.name }}, {{this.queryCity.country}}</div>
            <q-space/>
            <div v-if="unitSelection === 'metric'" class="text-h5 text-orange-10">{{ this.queryCity.temp }}&deg;C</div>
            <div v-else class="text-h5 text-orange-10">{{ this.queryCity.temp }}&deg;F</div>
          </div>
          <q-separator class="q-mb-md" color="dark"/>
          <div class="column text-center">
            <div class="self-center text-center text-no-wrap text-h5">{{ this.queryCity.description }}</div>
            <div class="text-center q-mb-lg">
              <q-img :src="this.queryCity.iconURL" alt="Weather icon" width="12%" class="self-center"/>
            </div>
          </div>
          <div class="row">
            <div class="text-h6 text-weight-regular">High: {{ this.queryCity.maxTemp }}</div>
            <q-space/>
            <div v-if="unitSelection === 'metric'" class="text-h6 text-weight-regular">Wind: {{ this.queryCity.wind }} &#13223;</div>
            <div v-else class="text-h6 text-weight-regular row">Wind: {{ this.queryCity.wind }} <div class="text-italic">mph</div></div>
          </div>
          <div class="row">
            <div class="text-h6 text-weight-regular">Low: {{ this.queryCity.minTemp }}</div>
            <q-space/>
            <div class="text-h6 text-weight-regular">Humidity: {{ this.queryCity.humidity }}%</div>
          </div>
        </q-card-section>
      </q-card>
    </div>
    <div v-else-if="!queryEntered" class="text-center">
      <q-img src="https://media.giphy.com/media/Js8fMtFd8ZZUQbTXzy/giphy.gif" alt="Happy Summer Sticker By Odsanyu" width="25%" />
      <div class="text-h5 text-italic text-grey-6">Search for a location to check the weather!</div>
    </div>
    <div v-else class="text-center">
      <q-img src="https://media.giphy.com/media/Mb4gjv0pVz7yuJPwNG/giphy.gif" alt="Happy Summer Sticker By Odsanyu" width="25%" class="q-mt-lg" />
      <div class="text-h5 text-italic text-grey-6 q-mt-md">Oops, I couldn't find your city. Check for typos!</div>
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
      queryEntered: false,
      queryCity: {
        name: null,
        country: null,
        temp: null,
        minTemp: null,
        maxTemp: null,
        description: null,
        iconURL: null,
        wind: null,
        humidity: null
      },
      queryError: false
    }
  },
  methods: {
    getWeatherData () {
      this.queryError = false
      this.queryEntered = true
      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=${this.unitSelection}&appid=${this.weatherAPI.key}`)
        .then((res) => {
          this.queryCity.name = res.data.name
          this.queryCity.country = res.data.sys.country
          this.queryCity.temp = res.data.main.temp
          this.queryCity.description = res.data.weather[0].description.charAt(0).toUpperCase() + res.data.weather[0].description.slice(1)
          this.queryCity.iconURL = `https://openweathermap.org/img/wn/${res.data.weather[0].icon}@2x.png`
          this.queryCity.minTemp = res.data.main.temp_min
          this.queryCity.maxTemp = res.data.main.temp_max
          this.queryCity.wind = res.data.wind.speed
          this.queryCity.humidity = res.data.main.humidity
        })
        .catch(() => {
          this.queryError = true
        })
    },
    unitsChange () {
      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=${this.unitSelection}&appid=${this.weatherAPI.key}`)
        .then((res) => {
          this.queryCity.temp = res.data.main.temp
          this.queryCity.minTemp = res.data.main.temp_min
          this.queryCity.maxTemp = res.data.main.temp_max
          this.queryCity.wind = res.data.wind.speed
        })
    }
  }
}
</script>
