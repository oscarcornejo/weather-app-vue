<template>
  <div class="app" :class="Object.keys(weather).length > 0 && weather.main.temp > 16 ? 'warm' : ''">
    <main>
      <InputSearch @getValue="getValue" />
      <DataWeather :weather="weather" />
    </main>
  </div>
</template>

<script>
import InputSearch from './components/InputSearch';
import DataWeather from './components/DataWeather';

export default {
  name: 'App',
  components: {
    InputSearch,
    DataWeather,
  },
  data() {
    return {
      api_key: process.env.VUE_APP_API_KEY,
      url_base: 'https://api.openweathermap.org/data/2.5/',
      termino: '',
      weather: {},
    };
  },
  methods: {
    getValue({ event, value }) {
      this.termino = value;
      this.fetchWeather(event, value);
    },
    async fetchWeather(e, value) {
      const urlApi = `${this.url_base}weather?q=${this.termino}&units=metric&APPID=${this.api_key}`;
      if (e.key == 'Enter') {
        await fetch(urlApi)
          .then((res) => res.json())
          .then((data) => this.setResults(data));
      }
    },
    async setResults(results) {
      this.weather = results;
    },
  },
};
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'montserrat', sans-serif;
}

.app {
  background-image: url('./assets/cold-bg.jpeg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;

  &.warm {
    background-image: url('./assets/warm-bg.jpeg');
  }
}

main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}
</style>
