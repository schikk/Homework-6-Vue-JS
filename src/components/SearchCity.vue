<template>
  <div class="weather-wrapper">
    <div class="weather-container">
      <h2>City to search</h2>
      <form id="city_search" @submit.prevent="loadWeather">
        <div class="form-wrapper">
          <input type="text" class="search-input" id="city" name="city" v-model="city" />
          <select name="country" id="country_select" v-model="country">
            <option value="ua">UA</option>
            <option value="pl">PL</option>
            <option value="ru">RU</option>
          </select>
          <button class="search-btn" type="submit">Show weather</button>
        </div>
      </form>
    </div>
    <div class="weather-container">
      <h2>Forecast</h2>
      <h3>{{ cityName }} {{ countryName }}</h3>
      <ul class="weather">
        <li class="weather__items" v-for="(weatherItems, index) in weatherItems" :key="index">
          <span class="weather-date">{{ weatherItems.dt_txt }}</span>
          <div class="weather__container">
            <div class="icon">
              <img
                v-bind:src="'http://openweathermap.org/img/w/' + weatherItems.weather[0].icon + '.png' "
              />
            </div>
            <span class="weather-temp">
              Temp min:
              <span
                class="weather-temp__text"
              >{{ kelToCel(weatherItems.main.temp_min) }}&deg; C</span>
            </span>
            <span class="weather-temp">
              Temp max:
              <span
                class="weather-temp__text"
              >{{ kelToCel(weatherItems.main.temp_max) }}&deg; C</span>
            </span>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from "axios";
// import VueAxios from "vue-axios";
export default {
  data: () => ({
    country: "ua",
    city: "",
    cityName: "",
    countryName: "",
    weatherItems: ""
  }),
  methods: {
    kelToCel(kelvin) {
      var celsius = Math.round(kelvin - 273.15);
      return celsius;
    },
    loadWeather() {
      const key = "a6d4f3187082b4cf8a4ff4e7ce34595b";
      const url = "https://api.openweathermap.org/data/2.5/forecast";
      const params = {
        appId: key,
        q: `${this.city},${this.country}`
      };
      axios
        .get(url, { params })
        .then(response => {
          this.cityName = response.data.city.name;
          this.countryName = response.data.city.country;
          this.weatherItems = response.data.list;
        })
        .catch(error => {
          alert(error);
          this.errored = true;
        })
        .finally(() => (this.loading = false));
    }
  }
};
</script>

<style>
.form-wrapper {
  display: flex;
  justify-content: center;
  font-size: 15px;
  letter-spacing: 1px;
}
.search-input {
  display: block;
  width: 200px;
  padding: 5px;
  border: 2px solid #000;
  margin-right: 30px;
}
#country_select {
  width: 50px;
  padding: 2px;
  border: 2px solid #000;
  margin-right: 30px;
}
.search-btn {
  display: block;
  background: #fff;
  border: 2px solid #000;
  padding: 10px 20px;
  border-radius: 20px;
  cursor: pointer;
}
.weather {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
.weather__items {
  width: 25%;
  list-style: none;
  margin: 10px;
}
.weather__container {
  padding: 10px;
  border: 1px solid #000;
}
.weather-date {
  display: block;
  margin-bottom: 10px;
}
.weather__container > span {
  display: block;
}
.weather-temp__text {
  font-weight: bold;
}
.weather-temp:not(:last-child) {
  margin-bottom: 10px;
}
</style>