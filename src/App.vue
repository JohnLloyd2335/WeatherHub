<script setup>
import { ref, reactive } from 'vue';
import axios from "axios";

const data = ref({})
const outputContainerStatus = ref(false)
const city = reactive({
  name: ""
})

const errors = ref("");
const errorContainerStatus = ref(false)
const errorImageURL = ref('src/assets/images/error.png')
const appIcon = ref('src/assets/images/weather.png')


const getWeatherData = async (cityname) => {
  try {
    const response = await axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${cityname},PH,63&limit=1&appid=f3adc98f6ce3c8dea8f26b40d4b08252`);
    data.value = response.data;
    outputContainerStatus.value = true;
    errorContainerStatus.value = false
  
  } catch (error) {
    errorContainerStatus.value = true;
    outputContainerStatus.value = false;
    data.value = {};
    if(error.response.status == 404){
      errors.value = "City Not Found"
    }
    else{
      errors.value = "Opps,There was an Error"
    }
  }
}

const getIconUrl = (iconCode) => {
  return `https://openweathermap.org/img/wn/${iconCode}@4x.png`;
};
</script>

<template>
  <header>
    <div class="container-w-100">
      <img :src="appIcon" alt="">
      <h1>WeatherHub</h1>
    </div>
  </header>
  <form @submit.prevent="getWeatherData(city.name)">
    <div class="container">
      <h1 id="text-label">Enter City</h1>

      <div class="input-container">

        <input type="text" v-model="city.name" required autocomplete="off">
        <button id="searchButton"><svg xmlns="http://www.w3.org/2000/svg" height="1em" viewBox="0 0 512 512">
            <path
              d="M416 208c0 45.9-14.9 88.3-40 122.7L502.6 457.4c12.5 12.5 12.5 32.8 0 45.3s-32.8 12.5-45.3 0L330.7 376c-34.4 25.2-76.8 40-122.7 40C93.1 416 0 322.9 0 208S93.1 0 208 0S416 93.1 416 208zM208 352a144 144 0 1 0 0-288 144 144 0 1 0 0 288z" />
          </svg>
        </button>

      </div>

      <div class="output-container" v-if="outputContainerStatus" >
        <div class="header">
          <button type="button" id="closeButtonOutput" v-on:click="outputContainerStatus = false"><svg xmlns="http://www.w3.org/2000/svg" height="1em" viewBox="0 0 384 512"><path d="M376.6 84.5c11.3-13.6 9.5-33.8-4.1-45.1s-33.8-9.5-45.1 4.1L192 206 56.6 43.5C45.3 29.9 25.1 28.1 11.5 39.4S-3.9 70.9 7.4 84.5L150.3 256 7.4 427.5c-11.3 13.6-9.5 33.8 4.1 45.1s33.8 9.5 45.1-4.1L192 306 327.4 468.5c11.3 13.6 31.5 15.4 45.1 4.1s15.4-31.5 4.1-45.1L233.7 256 376.6 84.5z"/></svg></button>
        </div>
        <div class="weather-icon">
          <img :src="data.weather && data.weather.length ? getIconUrl(data.weather[0].icon) : ''" alt="">
        </div>
        <div class="text-group">
          <h1 id="cityName">{{ data.name }}</h1>
        </div>
        <div class="text-group">
          <h1 id="mainStatus">{{ data.weather[0].main }}</h1>
          <p id="celcius">{{ Math.round(data.main.temp - 273.15 )}} °C</p>
        </div>
      </div>

      <div class="error-container" v-if="errorContainerStatus">
        <button type="button" id="closeButtonOutputErrors" v-on:click="errorContainerStatus = false"><svg xmlns="http://www.w3.org/2000/svg" height="1em" viewBox="0 0 384 512"><path d="M376.6 84.5c11.3-13.6 9.5-33.8-4.1-45.1s-33.8-9.5-45.1 4.1L192 206 56.6 43.5C45.3 29.9 25.1 28.1 11.5 39.4S-3.9 70.9 7.4 84.5L150.3 256 7.4 427.5c-11.3 13.6-9.5 33.8 4.1 45.1s33.8 9.5 45.1-4.1L192 306 327.4 468.5c11.3 13.6 31.5 15.4 45.1 4.1s15.4-31.5 4.1-45.1L233.7 256 376.6 84.5z"/></svg></button>
        <div class="error-message">
          <img :src="errorImageURL" alt="">
          <h1>{{ errors }}</h1>
        </div>

      </div>

    </div>
  </form>

  <footer>
    <h3>Developed by <span>John Lloyd Lacadin</span></h3>
  </footer>
</template>

<style scoped>
.container-w-100{
  padding: 10px 30px;
  display: flex;
  align-items: center;
  justify-content: start;
  background-color: rgb(205, 97, 58);
}
.container-w-100 h1{
  font-weight: bold;
  color: #ffffff;
  font-size: 40px;
}

.container-w-100 img{
  height: 50px;
  width: 50px;
  margin-right: 10px;
}
.container {
  height: 100vh;
  width: 100vw;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}

.container #text-label {
  font-weight: 800;
  color: #ffff;
  font-size: 40px;
}

.container .input-container {
  display: flex;
  gap: 10px;
  align-items: center;
  justify-content: center;
  margin-top: 10px;
  padding: 10px 5px;
}

.container input[type="text"] {
  width: 400px;
  height: 60px;
  outline: none;
  border: none;
  font-size: 35px;
  padding: 10px 0 10px 8px;
  border-radius: 8px;
}

.container button {
  height: 100%;
  width: 60px;
  outline: none;
  border: none;
  background-color: rgb(205, 97, 58);
  font-size: 22px;
  color: #ffffff;
  border-radius: 8px;
  cursor: pointer;
}

#searchButton {
  fill: #ffffff
}

.container #searchButton:hover {

  background-color: rgb(171, 63, 24);
  color: #ffffff;
}

.output-container{
  margin-top: 50px;
  border: 1px solid;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  padding: 20px 0px;
  border-radius: 8px;
  background-color: #fff;
  color: rgb(171, 63, 24);
  border: none;
  outline: none;
}

.output-container .header{
  align-self: flex-end;
  margin-right: 20px;
  display: flex;
  align-items: center;
  justify-content: end;
}

.output-container .header button{
  background-color: transparent;
}
#closeButtonOutput{
  fill: rgb(171, 63, 24);
  font-weight: bold;
  font-size: 30px;
}



.output-container .weather-icon{
  display: flex;
  align-items: center;
  justify-content: center;
}
.output-container .weather-icon img{
  width: 500px;
}
.output-container .text-group{
  display: flex;
  align-items: center;
  justify-content: space-between;
  border: 1px solid #fff;
  width: 500px;
  padding: 5px 10px;
  font-weight: bold;
  padding: 0 30px;
}

#cityName{
  font-size: 50px;
  font-weight: 900;
}

#mainStatus{
  font-size: 50px;
  font-weight: 900;
}

#celcius{
  font-size: 40px;
  font-weight: 900;
}

.error-container {
  margin-top: 100px;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 900px;
  padding: 20px 3px;
  font-weight: bold;
  flex-direction: column;
  border-radius: 8px;
}

.error-container button{
  align-self: flex-end;
  background: transparent;
  color: #fff;

}
#closeButtonOutputErrors{
  fill: #fff;
  font-size: 30px;
}
.error-container .error-message{
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}

.error-container img{
  height: 415px;
  width: 415px;
}

.error-container h1{
  color: rgb(171, 63, 24);
  font-size: 35px;
  font-weight: 900;
}

footer{
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 10px 0;
  margin-bottom: 5px;
}
footer h3 span{
  color: rgb(171, 63, 24);

}
</style>
