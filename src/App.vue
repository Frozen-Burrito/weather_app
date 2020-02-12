<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : '' ">
    <main>
      <div class="search-box">
        <input 
          type="text" 
          class="search-bar" 
          placeholder="Search" 
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{Math.round(weather.main.temp)}}°c</div>
          <div class="weather">{{weather.weather[0].main}}</div>
        </div>
      </div>
    </main>     
  </div>
</template>

<script>

export default {

  name: 'App',
  data () {
    return {
      
      api_key: 'eff70d1646c49425e23127f5b51c06cb',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },

  methods: {
    fetchWeather (e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },

    setResults(results) {
      this.weather = results;
    },

    dateBuilder() {
      let today = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

      let day = days[today.getDay()];
      let date = today.getDate();
      let month = months[today.getMonth()];
      let year = today.getFullYear();

      return `${day} ${date}, ${month} ${year}`;
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
    font-family: 'Montserrat', sans-serif;
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

    background-image: linear-gradient(to bottom, rgba(0,0,0,0.2), rgba(0,0,0,0.75));
  }

  .search-box {
    width: 100%;
    margin-bottom: 30px;
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

    box-shadow: 0px 0px 8px rgba(0,0,0,0.2);
    background-color: rgba(255,255,255,0.4);
    border-radius: 0px 16px 0px 16px;

    transition: 0.4s;
  }

  .search-box .search-bar:focus {
    box-shadow: 0px 0px 16px rgba(0,0,0,0.2);
    background-color: rgba(255,255,255,0.7);
    border-radius: 16px 0px 16px 0px;
  }

  .location-box .location {
    color: white;
    font-size: 32px;
    font-weight: 500;
    text-align: center;
    text-shadow: 1px 3px rgba(0,0,0,0.2);
  }

  .location-box .date {
    color: rgb(228, 228, 228);
    font-size: 20px;
    font-weight: 300;
    text-align: center;
  }

  .weather-box {
    text-align: center;
  }

  .weather-box .temp {
    display: inline-block;
    padding: 10px 25px;

    color: #fff;
    font-size: 102px;
    font-weight: 900;

    text-shadow: 3px 6px rgba(0, 0, 0, 0.2);
    background-color: rgba(255, 255, 255, 0.2);
    border-radius: 16px;
    margin: 30px 0;

    box-shadow: 3px 6px rgba(0, 0, 0, 0.2);
  }

  .weather-box .weather {
    color: rgb(228, 228, 228);
    font-size: 48px;
    font-weight: 700;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.2);
  }
</style>
