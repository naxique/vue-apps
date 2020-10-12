<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 15 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input 
          type="text" 
          class="search-bar" 
          :placeholder="errorHandling > 0 ? 'Search...' : 'City not found.'"
          v-model="searchQuery"
          @keyup.enter="doSearch"
        />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ date() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  name: 'App',
  setup () {
    const apiKey = 'f9a886a2be5bb97ae43f5c7459ae45af';
    const urlBase = 'https://api.openweathermap.org/data/2.5/';
    const searchQuery = ref('');
    const weather = ref([]);
    const errorHandling = ref(1)

    function doSearch() {
      fetch(`${urlBase}weather?q=${searchQuery.value}&units=metric&APPID=${apiKey}`)
        .then(r => { return r.json() }).then(setWeather)
    } function setWeather(result) { 
        weather.value = result; 
        searchQuery.value = ''; 
        if (weather.value.cod > 400) { errorHandling.value = 0 } else { errorHandling.value = 1 }
      }

    function date() {
      let d = new Date();
      let day = d.getDate();
      let month = d.getMonth()+1;
      let year = d.getFullYear();

      return `${day}.${month}.${year}`
    }

    return {
      apiKey,
      urlBase,
      searchQuery,
      doSearch,
      weather,
      date,
      errorHandling
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
}

#app {
  background-image: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm {
  background-image: url('./assets/warm-bg.jpg');
}

main {
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(to bottom, rgba(0,0,0, 0.25), rgba(0,0,0, 0.75));
}

.search-box {
  width: 100%;
  margin-bottom: 30px;

  transition: 0.4s;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  
  color: #313131;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0,0,0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 10px 10px 10px 10px;

  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0,0,0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);

  transition: 0.4s;
}

.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0,0,0, 0.25);

  transition: 0.4s;
}

.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
  text-shadow: 1px 3px rgba(0,0,0, 0.25);

  transition: 0.4s;
}

.weather-box {
  text-align: center;

  transition: 0.4s;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0,0,0, 0.25);
  background-color: rgba(255,255,255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 0px 0px 8px  rgba(0,0,0, 0.25);

  transition: 0.4s;
}

.weather-box .weather {
  color: #FFF;
  font-size: 48px;
  font-style: italic;
  font-weight: 700;
  text-shadow:1px 3px  rgba(0,0,0, 0.25);

  transition: 0.4s;
}
</style>
