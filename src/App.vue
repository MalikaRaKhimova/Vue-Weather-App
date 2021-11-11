<template>
  <div id="app" v-bind:class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'hot' : '' ">
    <main>
      <div class="search-box">
        <input 
        type="text" 
        class="search-bar" 
        placeholder="Enter city name "
        v-model="query"
        @keypress="fetchWeather">
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined' ">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{weather.sys.country}}</div>
          <div class="date">{{dateBuilder()}}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{Math.round(weather.main.temp)}}<span></span>C</div>
          <div class="weather">{{weather.weather[0].main}}</div>
          <img class="weather-icon" src="./assets/sunny.svg" alt="">
          <!-- <img class="weather-icon" v-bind:src="typeof weather.main != 'undefined' ? ((weather.main.temp) > 16 ?  : cloud): '' " alt=""> -->
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
      api_key: '69476badd18b8ea3657c204970ad41df',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather : {},
      sun: './assets/sunny.svg',
      cloud: './assets/cloudy.svg',
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
    setResults (results) {
      this.weather = results;
    },
    dateBuilder () {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June",
                      "July", "August", "September", "October", "November", "December"];
      let days = ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    },
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
  background-position: center;
  transition: .4s;
}

#app.hot {
  background-image: url('./assets/hot-bg.jpg');
}

#app.rain {
  background-image: url('./assets/rainy-bg.jpg');
  background-position: bottom;
  background-size: cover;
}

#app.cloud {
  background-image: url('./assets/wet-bg.jpg');
}


main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}

.search-box input {
  font-family: Montserrat;
  font-size: 16px;
  font-weight: 500;
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
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  box-shadow: 0 0 8px rgba(0, 0, 0, 0.25);
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0 0 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0 16px 0;
}

.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  transition-duration: 0.s;
}

.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  text-align: center;
  font-style: italic;
}
.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 50px 55px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 50px 0;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.temp span {
  display: inline-block;
  border: 9px solid #fff;
  border-radius: 50%;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background: none;
  margin: 0 5px;
  vertical-align: top;
  width: 35px;
  height: 35px;
  margin-top: 15px;
}


.weather-box .weather-icon {
  margin-top: 30px;
  width: 100px;
  height: 100px;
}
</style>
