<template>
    <div class="app">
      <div class="container1">
        <h3 style="text-align: center">Weather App</h3>
        <hr />
        <div class="form__group field">
          <input
            type="input"
            class="form__field"
            placeholder="Enter City Name"
            v-model="city"
            @keyup.enter="check"
          />
          <label for="name" class="form__label">City Name</label>
        </div>
        <button class="shadow__btn" @click="check" >Check</button>
      </div>
  
      <div class="container2" v-if="isAvailable">
        <div class="degree">
          <img :src="iconURL" alt="icon" />
          <br>
          {{ temperature }}° C
          <br>
          <div class="descript">{{ description }}</div>
          <h5>{{ location }}</h5>
        </div>
      </div>
    </div>
  </template>
  
  <script lang="ts">
  import { defineComponent, ref, onMounted } from "vue";
  import axios from "axios";
  
  export default defineComponent({
    name: "App",
    components: {},
    setup() {
      const isAvailable = ref(false);
      const API_KEY = ref("17018eb2069738a1ad3a10139a80eb7c")
      const city = ref("");
      const statusCode = ref<number>(0);
      const weatherData = ref<any>(null);
      const temperature = ref();
      const icon = ref("");
      const iconURL = ref("");
      const description= ref("")
      const location= ref("")
  
      const check = async () => {
        isAvailable.value = true;
        try {
          const response = await axios.get(
            "https://api.openweathermap.org/data/2.5/weather",
            {
              params: {
                q: city.value,
                appid: API_KEY.value,
              },
            }
          );
          statusCode.value = response.status;
          weatherData.value = response.data;
        } catch (error) {
          alert("error fetching weather data");
        }
  
        temperature.value = Math.floor(weatherData.value.main.temp - 273);
        icon.value = weatherData.value.weather[0].icon;
        description.value = weatherData.value.weather[0].main;
        location.value= weatherData.value.name //"Dhaka" name
  
        iconURL.value = "http://openweathermap.org/img/w/" + icon.value + ".png";
      };
      return {
        isAvailable,
        check,
        weatherData,
        temperature,
        city,
        icon,
        iconURL,
        description,
        location
      };
    },
  });
  </script>
  
  <style>
  @import url('https://fonts.googleapis.com/css2?family=League+Spartan&display=swap');
  body {
    background-color: #006494;
    font-family: 'League Spartan', sans-serif;
  }
  .container1 {
    background: #f1f7fe;
    overflow: hidden;
    border-radius: 16px;
    color: #010101;
  
    position: absolute;
    width: 20%;
    height: 40%;
    padding: 20px;
    /* margin: 20px; */
    top: 40%;
    left: 20%;
    /* transform: translate(-50%, -50%); */
    flex-direction: column;
    overflow-y: auto;
    /* Your container styles */
  }
  .container2 {
    background: #f1f7fe;
    overflow: hidden;
    border-radius: 16px;
    color: #010101;
  
    position: absolute;
    max-width: 30%;
    min-width: 30%;
    height: 70%;
    padding: 20px;
    /* margin: 20px; */
    top: 20%;
    left: 50%;
    /* transform: translate(-50%, -50%); */
  
    flex-direction: column;
    overflow-y: auto;
    /* Your container styles */
  
   
  }
  .degree {
    font-family: Verdana;
    font-size: 60px;
    color: #27374d;
    text-align: center;
  }
  img {
    height: 80px;
    width: 80px;
  }
  .descript{
    font-size: 30px;
  }
  /* input design */
  .form__group {
    position: relative;
    padding: 20px 0 0;
    width: 100%;
    max-width: 180px;
    margin-bottom: 80px;
  }
  
  .form__field {
    font-family: inherit;
    width: 100%;
    border: none;
    border-bottom: 2px solid #9b9b9b;
    outline: 0;
    font-size: 17px;
    color: #27374d;
    padding: 7px 0;
    background: transparent;
    transition: border-color 0.2s;
  }
  
  .form__field::placeholder {
    color: transparent;
  }
  
  .form__field:placeholder-shown ~ .form__label {
    font-size: 17px;
    cursor: text;
    top: 20px;
  }
  
  .form__label {
    position: absolute;
    top: 0;
    display: block;
    transition: 0.2s;
    font-size: 17px;
    color: #27374d;
    pointer-events: none;
  }
  
  .form__field:focus {
    padding-bottom: 6px;
    font-weight: 700;
    border-width: 100%;
    border-image: linear-gradient(to right, #116399, #27374d);
    border-image-slice: 1;
  }
  
  .form__field:focus ~ .form__label {
    position: absolute;
    top: 0;
    display: block;
    transition: 0.2s;
    font-size: 17px;
    color: #27374d;
    font-weight: 700;
  }
  
  /* reset input */
  .form__field:required,
  .form__field:invalid {
    box-shadow: none;
  }
  
  .shadow__btn {
    padding: 10px 20px;
    border: none;
    font-size: 17px;
    color: #fff;
    border-radius: 7px;
    letter-spacing: 4px;
    font-weight: 700;
    text-transform: uppercase;
    transition: 0.5s;
    transition-property: box-shadow;
    width: 100%;
  }
  
  .shadow__btn {
    background: rgb(0, 140, 255);
    box-shadow: 0 0 25px rgb(0, 140, 255);
  }
  
  .shadow__btn:hover {
    box-shadow: 0 0 5px rgb(0, 140, 255), 0 0 25px rgb(0, 140, 255),
      0 0 50px rgb(0, 140, 255), 0 0 100px rgb(0, 140, 255);
  }
  </style>
  