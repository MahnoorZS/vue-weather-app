<template>
 <!-- <div class="home">
        <h2>The 
           App</h2>
        <button id="searchbtn" type="button" onClick="showWeatherComponent()">Search forecast.</button>
  </div> -->
          <!-- <button id="searchbtn" type="button" v-on:click="showWeatherComponent()">Search forecast.</button> -->

  <div id="app"  :class="weatherClass">
    <!-- <div id="app" :class="typeof weather.main != 'undefined' && weather.weather[0].main == 'Rain' ? 'rain' : ''"> -->
  <!-- weather.weather[0].main == 'Smoke' ? 'smoke' : weather.weather[0].main == 'Clouds' ? 'clouds' : weather.weather[0].main == 'Snow' ? 'snow' :  -->
    <main>
      <h2>The Weather App</h2>
       <!-- <button id="searchbtn" type="button" v-on:click="showWeatherComponent()">Search forecast.</button> -->
<!-- serach/display weather page -->
      <div class="search-box">
        <input type="text" 
               class="search-bar"
               placeholder="Search..."
               v-model="query"
               @keypress="fetchWeather"
               style="visibility: visible;"> 
               <!-- binding the query with v-model -->
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
          <div class="date">
            {{ dateBuilder() }}
          </div>
          <div class="weather-box">
            <div class="temp">{{ Math.round(weather.main.temp) }} °C</div>
            <div class="weather">{{ weather.weather[0].main }}</div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>


<script>


export default {
  name: 'app',
  // return the key
  data () {
    return {
      api_key: 'd678d955969c2428608445d521242ae2',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {},
      weatherClass: ''
    }
  },

  methods: {
    fetchWeather(e){
      if(e.key == "Enter"){
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`) //allows us to make requests from the api
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },

    setResults(results){
      this.weather = results;
      console.log(this.weather)
      if(typeof this.weather.main != 'undefined' && this.weather.weather[0].main == 'Clear'){
        this.weatherClass = 'clear';
      }
      else if(typeof this.weather.main != 'undefined' && this.weather.weather[0].main == 'Clouds'){
        this.weatherClass = 'clouds';
      }
      else if(typeof this.weather.main != 'undefined' && this.weather.weather[0].main == 'Rain'){
        this.weatherClass = 'rain';
      }
      else if(typeof this.weather.main != 'undefined' && this.weather.weather[0].main == 'Snow'){
        this.weatherClass = 'snow';
      }
      else if(typeof this.weather.main != 'undefined' && this.weather.weather[0].main == 'Smoke'){
        this.weatherClass = 'smoke';
      }
      else if(typeof this.weather.main != 'undefined' && this.weather.weather[0].main == 'Haze'){
        this.weatherClass = 'haze';
      }
      else{
        this.weatherClass = '';
      }
    },

    dateBuilder () {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },

    // showWeatherComponent(){
    //   // console.log("hello");
    // var weatherComponent = document.getElementsByClassName("search-bar");
    //    if(weatherComponent.style.visibility === "visible"){
    //     weatherComponent.style.visibility= "hidden";
    //    }
    //   else{
    //     console.log("hello");
    // //      weatherComponent.style.display = "none";
    //    }
    // }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

h2{
  font-family: Georgia, 'Times New Roman', Times, serif;
  color:blanchedalmond;
  font-style: bold;
  font-size: 70px;
  text-align: center;
  display: block;
  margin-bottom: 20px;
}

body {
  font-family: 'Cambria, serif';
  height: 100%;
}

#app {
  background-image: url('./assets/4seasons.jpg');
  background-size: cover;
  background-position: center;
  transition: 0.4s;
  background-repeat: no-repeat;
  height: 100%;
  width: 100%;
}

#home{
height: 100%;
}

#app.clear {
  background-image: url('./assets/sunny-gif.gif'); ;
}

#app.rain {
  background-image: url('./assets/rain-gif.gif'); ;
}

#app.clouds {
  background-image: url('./assets/clouds-gif.gif'); ;
}

#app.snow {
  background-image: url('./assets/snow-gif.gif'); ;
}

#app.smoke {
  background-image: url('./assets/smoke-gif.gif'); ;
}

#app.haze {
  background-image: url('./assets/haze-gif.gif'); ;
}

main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0,0,0,0.75));
}

.search-box{
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar{
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  
  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0,0,0,0.25);
  background-color: rgba(255, 255, 255, 0.75) ;
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
   box-shadow: 0px 0px 16px rgba(0,0,0,0.25);
   background-color: rgba(255, 255, 255, 1) ;
   border-radius: 16px 0px 16px 0px;   
}

.location-box .location {
  color: #FFF;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: #FFF;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #FFF;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color:rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: #FFF;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
