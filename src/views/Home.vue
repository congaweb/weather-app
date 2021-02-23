<template>
  <main class="container">
    <h2>현재 {{ convertCountry }} 날씨 정보</h2>
    <div class="region-list">
      <button type="button" @click="getWeather('seoul')">서울</button>
      <button type="button" @click="getWeather('busan')">부산</button>
    </div>
    <div class="info-group">
      <p class="weather">{{ status }}</p>
      <p class="temp">{{ convertTemp }} ℃</p>
      <p class="sensory-temp">체감온도 : {{ convertFeelsTemp }} ℃</p>
      <p>기압 : {{ pressure }}</p>
      <p>가시거리 : {{ visibility }}km</p>
    </div>
  </main>
</template>

<script>
import axios from "axios";

export default {
  name: "contents",
  data(){
    return{
      baseUrl:"https://api.openweathermap.org/data/2.5/weather?",
      keyValue:"5d10b56c72a223d178342aaef3d01c42",
      country:"",
      temperature:0,
      icon:"",
      status:"",
      pressure:"",
      feelsLike:"",
      visibility:"",
    }
  },
  async created(){
    let baseCountry = "seoul";
    const response = await axios.get(
        `${this.baseUrl}q=${baseCountry}&appid=${this.keyValue}`
    );
    const data = response.data;
    this.temperature = data.main.temp;
    this.pressure = data.main.pressure;
    this.feelsLike = data.main.feels_like;
    this.icon = data.weather[0].icon;
    this.status = data.weather[0].main;
    this.visibility = data.visibility * 0.001;
    this.country = baseCountry;
  },
  computed: {
    weatherIcon(){
      return `http://openweathermap.org/img/w/${this.icon}.png`;
    },
    convertTemp() {
      let value = this.temperature;
        value = Math.round((value - 273.15) * 10) / 10; // 절대온도 to 섭씨온도
      return value;
    },
    convertFeelsTemp() {
      let value = this.feelsLike;
      value = Math.round((value - 273.15) * 10) / 10; // 절대온도 to 섭씨온도
      return value;
    },
    convertCountry() {
      return this.country == "seoul" ? "서울" : "부산";
    }
  },
  methods: {
    async getWeather(baseCountry) {
      const response = await axios.get(
          `${this.baseUrl}q=${baseCountry}&appid=${this.keyValue}`
      );
      const data = response.data;
      this.temperature = data.main.temp;
      this.pressure = data.main.pressure;
      this.feelsLike = data.main.feels_like;
      this.icon = data.weather[0].icon;
      this.status = data.weather[0].main;
      this.visibility = data.visibility * 0.001;
      this.country = baseCountry;
    }
  }
}
</script>

<style scoped>

</style>