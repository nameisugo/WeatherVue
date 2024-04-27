<script>
import axios from 'axios'

export default {
  data() {
    return {
      city: "",
      error: "",
      info: null
    }
  },
  computed: {
    cityName() {
      return "'" + this.city + "'"
    },
    showTemp() {
      return "Температура: " + this.info.main.temp
    },
    showFeelsLike() {
      return "Ощущается как: " + this.info.main.feels_like
    },
    showMinTemp() {
      return "Минимальная температура: " + this.info.main.temp_min
    },
    showTempMAx() {
      return "Максимальная температура: " + this.info.main.temp_max
    },
  },
  methods: {
    getWeather() {
      if(this.city.trim().length < 2) {
          this.error = "Нужно название более одного символа"
          return false
      }

      this.error = ""

      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=e1eb2417d9e81c0233d0f73cc3f8809e`)
        .then(res => (this.info = res.data))
    }
  }
}
</script>


<template>
  <div class="wrapper">
    <h1>Weather app</h1>
    <p>Узнай погоду в {{ city == "" ? "вашем городе!" : cityName }}</p>
    <input type="text" v-model="city" placeholder="Введите город">
    <button v-if="city != ''" @click="getWeather()">Получить прогноз</button>
    <button disabled v-else>Введите название города</button>
    <p class="error">{{ error }}</p>

    <div class="info" v-if="info != null">
      <p>{{ showTemp }}</p>
      <p>{{ showFeelsLike }}</p>
      <p>{{ showMinTemp }}</p>
      <p>{{ showTempMAx }}</p>
    </div>
  </div>
</template>


<style scoped>
  .info {
    background: rgb(60, 60, 163);
    width: 400px;
    border-radius: 50px;
    margin-top: 50px;
    margin-left: 250px;
  }

  .error {
    color: orangered;
  }

  .wrapper {
    width: 900px;
    height: 500px;
    border-radius: 50px;
    padding: 20px;
    background: rgb(26, 88, 46);
    text-align: center;
    color: white;
  }

  .wrapper h1 {
    margin-top: 50px;
  }

  .wrapper p {
    margin-top: 25px;
  }

  .wrapper input {
    margin-top: 30px;
    background: transparent;
    border: 0;
    border-bottom: 2px solid rgb(26, 22, 22);
    color: aliceblue;
    font-size: 14px;
    padding: 5px 8px;
    outline: none;
  }

  .wrapper input:focus {
    border-bottom-color: rgb(255, 181, 92);
  }

  .wrapper button {
    background: rgb(246, 179, 86);
    color: antiquewhite;
    border-radius: 10px;
    border: 2px solid orange;
    padding: 10px 15px; 
    margin-left: 20px;
    cursor: pointer;
    transition: transform 500ms ease;
  }

  .wrapper button:hover {
    transform: scale(1.1) translateY(-5px);
  }

  .wrapper button:disabled {
    background: rgb(115, 91, 58);
    cursor: not-allowed;
  }
</style>
