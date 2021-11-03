<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <main>
        <div class="search-box">
            <input 
            type="text" 
            class="search-bar" 
            placeholder="Search..."
            v-model="query"
            @keypress="fetchWeather"
            />
        </div>

        <div class="widget" v-if="typeof weather.main != 'undefined'">
            <div class="weatherIcon"></div>
            <div class="weatherData">
                <h1 class="temperature">{{ Math.round(weather.main.temp) }}°</h1>
                <h2 class="description">{{ weather.weather[0].main }}</h2>
                <h3 class="city">{{ weather.name }}, {{ weather.sys.country }}</h3>
            </div>
            <div class="date">
                <h4 class="month" id="month">{{ dateBuilder() }}</h4>
            </div>
        </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      api_key: process.env.MIX_VUE_APP_OPENWEATHERMAP,
      url_base: process.env.MIX_VUE_APP_URL_OPENWEATHERMAP,
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
            })
            .then(this.setResults);
      }
    },
    setResults (results) {
        this.weather = results;
    },
    dateBuilder () {
        let d = new Date();
        let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
        let date = d.getDate();
        let month = months[d.getMonth()];

        return `${date} ${month}`;
    }
  }
}
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'montserrat', sans-serif;
}

main {
  min-height: 100vh;
  padding: 25px;
  background: rgb(2,0,36);
  background: linear-gradient(90deg, rgba(2,0,36,1) 0%, rgba(9,121,48,1) 35%, rgba(0,212,255,1) 100%);
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
  border:none;
  outline: none;
  background: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
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

@import url(https://fonts.googleapis.com/css?family=Open+Sans:300,400,700);
@import url(https://cdnjs.cloudflare.com/ajax/libs/weather-icons/1.2/css/weather-icons.min.css);

body {
	background: linear-gradient(90deg, #754d8b 0%, #4e5992 100%);
	font-size: 18px;
}

h1, h2, h3, h4, h5 {
	font-weight: normal;
	margin: 0;
	padding: 0;
}

.widget {
	height: 300px;
	width: 450px;
	position: absolute;
    left: 50%;
	top: 50%;
	transform: translate(-50%, -50%);
}

.weatherIcon{
	background-color: #f0f8fa;
	height: 70%;
	width: 100%;
	position: absolute;
    left: 0;
	top: 0;
	border-top-left-radius: 10px;
	border-top-right-radius: 10px;
}

.weatherIcon:before{
	display: block;
	content:"\f002";
	color: #818b8d;
	font-family: weathericons; 
	font-size: 110px;
	position: absolute;
	left: 50%;
	top: 58%;
	transform: translate(-50%, -50%);
}

.weatherData {
	background-color: #2e3336;
	height: 30%;
	width: 100%;
	position: absolute;
    left: 0;
	bottom: 0;
	border-bottom-left-radius: 10px;
	border-bottom-right-radius: 10px;
}

h1 {
	color: #c5cdcf;
	font-family: 'Open Sans';
  font-weight: 200;
	font-size: 2.9375em;
	line-height: 2.9375em;
	position: absolute;
	left: 6%;
	top: 50%;
	transform: translate(0, -50%);
}

h2 {
	color: #8f9b9d;
	font-family: 'Open Sans';
  font-weight: 200;
	font-size: 1.1875em;
	line-height: 1.1875em;
	position: absolute;
	top: 24%;
	left: 27%;
}

h3 {
	color: #c5cdcf;
	font-family: 'Open Sans';
  font-weight: 400;
	font-size: 0.8125em;
	line-height: 0.8125em;
	position: absolute;
	bottom: 25%;
	left: 27%;
}

h4 {
	color: #fff;
	font-family: 'Open Sans';
    font-weight: 700;
	text-transform: uppercase;
	font-size: 0.6875em;
	position: absolute;
	top: 30%;
	left: 50%;
	transform: translate(-50%, 0);
}

h5 {
	color: #fff;
	font-family: 'Open Sans';
  font-weight: 500;
	font-size: 1.8125em;
	line-height: 28px;
	position: absolute;
	bottom: 24%;
	left: 50%;
	transform: translate(-50%, 0);
}

.date {
	background-color: #4dbde2;
	height: 30%;
	width: 22%;
	position: absolute;
    right: 0;
	bottom: 0;
	border-bottom-right-radius: 10px;
}
</style>
