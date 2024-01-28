<template>
  <div class="d-flex justify-center mb-6 myCard ">
    <v-card class="w-25 myCard2 justify-center" title="Se vädret i din stad" variant="elevated" color="primary">

      <v-select class="w-75 ms-16" label="Välj stad" v-model="select" :items="citys" item-title="namn" variant="solo"
        @click="showWeather = false"></v-select>
      <div class="d-flex justify-center pb-5 mr-5">
        <v-btn @click="showTheWeather">
          Visa
        </v-btn>
      </div>
    </v-card>
  </div>
  <div v-if="showWeather" class="d-flex justify-center mb-6 ">
    <v-card class="w-25 myCard2" variant="elevated" color="primary">
      <div class="d-flex justify-center pt-5">
        <h1>{{ select }}</h1>
      </div>
      <div class="d-flex justify-center">
        <h1>TID: {{ time }}</h1>
      </div>
      <div class="d-flex justify-center pa-10">
        <v-img :width="280" aspect-ratio="1/1" cover :src="myPath"></v-img>
      </div>
      <div class="d-flex justify-center">
        <h1>Temperatur: {{ temp }} &#8451;</h1>
      </div>
      <div class="d-flex justify-center pb-5">
        <h2>{{ weatherSymbol }}</h2>
      </div>

    </v-card>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import axios from 'axios';


export default defineComponent({
  data() {
    return {
      select: null,
      citys: [{ id: 1, namn: "Göteborg", url: "https://opendata-download-metfcst.smhi.se/api/category/pmp3g/version/2/geotype/point/lon/11.9688/lat/57.7089/data.json" },
      { id: 2, namn: "Malmö", url: "https://opendata-download-metfcst.smhi.se/api/category/pmp3g/version/2/geotype/point/lon/13.0002/lat/55.5971/data.json" },
      { id: 3, namn: "Stockholm", url: "https://opendata-download-metfcst.smhi.se/api/category/pmp3g/version/2/geotype/point/lon/18.0622/lat/59.3276/data.json" },
      { id: 4, namn: "Sundsvall", url: "https://opendata-download-metfcst.smhi.se/api/category/pmp3g/version/2/geotype/point/lon/17.2909/lat/62.3911/data.json" }],
      showWeather: false,
      weatherData: null,
      temp: "",
      weatherSymbol: "",
      time: "",
      wnum: null,
      myPath: ""

    }
  },


  methods: {
    showTheWeather() {
      if (this.showWeather === false) {
        this.showWeather = !this.showWeather
        console.log(this.showWeather)
        this.getWeather()
      }

    },
    async getWeather() {
      if (this.select !== null) {

        const selectedLocationObject = this.citys.find(city => city.namn === this.select);

        if (selectedLocationObject) {

          const endpoint = selectedLocationObject.url;

          // Make the Axios GET request
          await axios.get(endpoint)
            .then(response => {
              // Handle the response data
              this.weatherData = response.data;
              console.log("DATA")
              console.log(this.weatherData)
            })
            .catch(error => {
              // Handle errors
              console.error('Error fetching data:', error);
            });
        }
        this.temp = this.weatherData.timeSeries[0].parameters[10].values[0]
        this.time = this.convertToHHMM(this.weatherData.timeSeries[0].validTime)
        this.wnum = this.weatherData.timeSeries[0].parameters[18].values[0]
        this.getWeatherSymbol(this.wnum)
      }
    },
    convertToHHMM(dateTimeString: string) {
      const date = new Date(dateTimeString);

      // Get hours and minutes
      const hours = date.getUTCHours().toString().padStart(2, '0');
      const minutes = date.getUTCMinutes().toString().padStart(2, '0');

      return `${hours}:${minutes}`;
    },
    getWeatherSymbol(Wsym) {
      if (Wsym === 3) {
        this.weatherSymbol = "Molnigt"
        this.myPath = new URL('../assets/mulet.png', import.meta.url).href
      }
      if (Wsym === 1) {
        this.myPath = new URL('../assets/soligt.png', import.meta.url).href
      }

    }
  }
})

//
</script>

<style scoped>
.myCard {
  margin-top: 100px;
}

.myCard2 {
  color: #676767;
}
</style>
