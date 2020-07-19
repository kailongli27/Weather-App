<template>
  <q-page class="column">
    <div class="column text-center bg-orange-13">
      <div class="text-h3 text-weight-light text-white q-mt-lg">
        Weather Check
      </div>
      <q-input rounded standout="bg-orange-14" v-model="text" placeholder="Enter a city name" dense class="q-mt-lg q-mb-sm self-center text-black" style="width: 35%">
        <template v-slot:prepend>
          <q-icon name="place"></q-icon>
        </template>
        <template v-slot:append>
          <q-icon name="search" @click="text = ''" class="cursor-pointer"></q-icon>
        </template>
      </q-input>
      <q-toggle
        v-model="third"
        checked-icon="check"
        color="green"
        unchecked-icon="clear"
        class="self-center"
      ></q-toggle>
    </div>
  </q-page>
</template>

<script>
export default {
  name: 'WeatherPage',
  data () {
    return {
      text: '',
      third: false,
      weatherKey: 'ad0ddaff0572e83a6fcf96cf7a2e3ade'
    }
  }
}
</script>
