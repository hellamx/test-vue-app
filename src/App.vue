<script>
import axios from "axios";

export default {
  data() {
    return {
      api_key: 'c04411a79ad707b4aa1361622f7fbb6b',
      city: '',
      error: '',
      info: null
    }
  },
  computed: {
    cityName() {
      return this.city.length > 0;
    },
    infoSuccess() {
      return this.info !== null;
    },
    showTemp() {
      return "Температура: " + this.info.temp;
    },
    showTempFeelsLike() {
      return "Ощущается как: " + this.info.feels_like;
    },
    showMinTemp() {
      return "Минимальная дневная температура: " + this.info.temp_min;
    },
    showMaxTemp() {
      return "Максимальная дневная температура: " + this.info.temp_max;
    },
    errorExists() {
      return this.error.length > 0;
    }
  },
  methods: {
    getWeather() {
      if (this.city.trim().length < 2) {
        this.error = 'Неккоретно заполнены данные';
        return false;
      }

      this.error = '';
      let $icon = document.querySelector('.wrapper');
      $icon.classList.remove('empty');

      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=${this.api_key}`)
          .then(result => (this.info = result.data.main));
    }
  }
}
</script>

<template>
    <div class="wrapper empty">
      <h1>Weather app</h1>
      <p>Погода в вашем городе <span class="city"></span></p>

      <p class="wrapper__show_city" v-if="cityName">Узнать погоду в <span> {{ "«" + city + "»" }} </span></p>

      <ul v-if="infoSuccess && cityName">
        <li>{{ showTemp }}</li>
        <li>{{ showTempFeelsLike }}</li>
        <li>{{ showMinTemp }}</li>
        <li>{{ showMaxTemp }}</li>
      </ul>

      <div class="form">
        <p class="error" v-if="errorExists">{{ error }}</p>
        <div class="form-container">
          <input type="text" v-model="city" placeholder="Введите город">
          <button type="button" v-if="!cityName" disabled>Получить прогноз</button>
          <button type="button" @click="getWeather()" v-else>Получить прогноз</button>
        </div>
      </div>
    </div>
</template>

<style lang="scss">
.city {
    font-weight: bold;
    transition: all .2s ease;
}

.error {
  display: block;
  width: 100%;
  color: #dc3545;
  padding-bottom: 7px;
  text-align: left;
}

.wrapper {
  position: relative;
  width: 900px;
  height: 500px;
  border-radius: 20px;
  padding: 20px;
  background: #e9e8e8;
  text-align: center;
  color: rgba(39, 55, 77, 1);
  font-family: Arial, Helvetica, sans-serif;

  ul {
    margin-top: 30px;
  }

  ul li {
    font-size: 1.8rem;
    padding: 7px 0;
  }
}

.empty::after {
  content: '';
  height: 80px;
  width: 80px;
  position: absolute;
  top: 50%;
  left: 50%;
  background-size: 80px 80px;
  background-image: url('@/assets/icons/empty.svg');
  background-repeat: no-repeat;
  transform: translate(-50%, -50%);
}

.wrapper__show_city span {
  font-weight: bold;
}

.wrapper h1 {
  font-size: 3.5rem;
  margin-top: 30px;
}

.wrapper p {
  margin-top: 5px;
  font-size: 1.6rem;
}

.form {
  margin-top: 20px;
  position: absolute;
  width: 90%;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
}

.form-container {
  display: flex;
}

.wrapper input[type="text"] {
  display: block;
  width: 75%;
  color: rgba(39, 55, 77, 1);
  padding: 7px 13px;
  font-size: 1.6rem;
  border-bottom: 3px solid rgba(39, 55, 77, 1);
  border-top: 1px solid rgba(39, 55, 77, 1);
  border-left: 1px solid rgba(39, 55, 77, 1);
  border-top-left-radius: 5px;
  border-bottom-left-radius: 5px;
}

.wrapper button {
  background: rgba(39, 55, 77, 1);
  padding: 10px 14px;
  display: block;
  width: 215px;
  font-size: 1.6rem;
  color: #e9e8e8;
  border-top-right-radius: 5px;
  border-bottom: 3px solid rgba(39, 55, 77, 1);
  border-bottom-right-radius: 5px;
  cursor: pointer;
}

.wrapper button:hover {
  transform: scale(1.04);
  transition: all .2s ease-out;
}

.wrapper button:disabled {
  background: #88909b;
  border-bottom: 3px solid rgba(39, 55, 77, 1);
}

.wrapper button:disabled:hover {
  transform: none;
}
</style>
