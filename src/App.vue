<template>
  <div id="app">
    <h1>Weather Widget</h1>
    <search-city />
  </div>
</template>

<script>
import SearchCity from "./components/SearchCity.vue";

export default {
  components: {
    SearchCity
  }
};
</script>

<style>
#app {
  width: 60%;
  margin: 0 auto;
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.weather-container {
  padding: 10px;
  border: 2px solid #e7e7e7;
  margin-bottom: 20px;
}
</style>
