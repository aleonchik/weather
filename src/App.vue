<script>
  import axios from 'axios';

  export default {
    data() {
      return {
        city: "",
        error: "",
        info: null,
        temp: null,
        feels_like: null,
        temp_min: null,
        temp_max: null,
        pressure: null
      }
    },
    computed: {
      cityName() {
        return "-=> " + this.city + " <=-";
      },
      
      showTemp() {
        return "Температура: " + this.temp
      },
      
      showFeelsLike() {
          return "Ощущается как: " + this.feels_like
      },

      showMinTemp() {
        return "Минимальная температура: " + this.temp_min
      },

      showMaxTemp() {
        return "Максимальная температура: " + this.temp_max
      },

      showPressure() {
        return "Давление: " + this.pressure
      }
    },
    methods: {
      getWeather() {
        if(this.city.trim().length < 2) {
          this.error = "Нужно название более одного символа"
          return false
        }

        this.error = ""

        axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=d9a7a6c355235f6e94260c0282a8dc27`)
          .then(res => (this.info = res.data))

        this.temp = this.info.main.temp
        this.feels_like = this.info.main.feels_like
        this.temp_min = this.info.main.temp_min
        this.temp_max = this.info.main.temp_max
        this.pressure = this.info.main.pressure
      }
    }
  }
</script>


<template>
  <div class="wrapper">
    <h1>Погодное приложение</h1> 
    <p>Узнать погоду в {{ city == "" ? "вашем городе" : cityName }}</p>
    <!-- <input type="text" v-on:input="this.city = $event.target.value" placeholder="Введите город"> -->
    <!-- <input type="text" @:input="this.city = $event.target.value" placeholder="Введите город"> -->
    <input type="text" v-model="city" placeholder="Введите город">
    <!-- <button v-show="city != ''">Получить погоду</button> -->
    <button v-if="city != ''" @click="getWeather()">Получить погоду</button>
    <button disabled v-else>Введите название гоорода</button>
    <p class="error">{{ error }}</p>

    <!-- <p v-show="info != null">{{ info.main }}</p> -->
    <!-- <p>{{ info.main }}</p> -->
    
    <div v-if="info != null">
      <p>{{ showTemp }}</p>
      <p>{{ showFeelsLike }}</p>
      <p>{{ showMinTemp }}</p>
      <p>{{ showMaxTemp }}</p>
      <p>{{ showPressure }}</p>
    </div>
  </div>
</template>


<style scoped>
  
  .error {
    color: #d03939;
  }

  .wrapper {
    width: 900px;
    height: 500px;
    border-radius: 50px;
    padding: 20px;
    background-color: #1f0f24;
    text-align: center;
    color: #fff;
  }

  .wrapper h1 {
    margin-top: 50px;
  }

  .wrapper p {
    margin-top: 20px;
  }

  .wrapper input {
    margin-top: 30px;
    background: transparent;
    border: 0;
    border-bottom: 2px solid #110813;
    color: #fcfcfc;
    font-size: 14px;
    padding: 5px 8px;
    outline: none;
  }

  .wrapper input:focus {
    border-bottom-color: #6e2d7d;
  }

  .wrapper button {
    background: #e3bc4b;
    color: #fff;
    border-radius: 10px;
    border: 2px solid #b99935;
    padding: 10px 15px;
    margin-left: 20px;
    cursor: pointer;
    transition: transform 500ms ease;
  }

  .wrapper button:hover {
    transform: scale(1.2, 1.2) translateY(-5px);
  }

  .wrapper button:disabled {
    background: #746027;
    cursor: not-allowed;
  }


</style>
