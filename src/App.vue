<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 289.15 ? 'warm' : ''">
      <main>
        <div class="search-box">
          <input type="text" class="search-bar" placeholder="Search..." v-model="query" @keypress="fetchWeather">
        </div>

        <div class="wearther-wrap" v-if="typeof weather.main != 'undefined'">
            <div class="location-box">
              <h2 class="location">{{weather.name}}, {{weather.sys.country}}</h2>
              <p class="date">{{dateBuilder()}}</p>
            </div>

             <div class="weather-box">
              <p class="temp">{{ Math.round(weather.main.temp - 273.15) }}°c</p>
              <p class="weather">{{weather.weather[0].main}}</p>
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
      api_key : '420c99091cd4078da54b964c3edddfca',
      url_base : 'https://api.openweathermap.org/data/2.5/',
      query : '',
      weather : {}
    }
  },
  methods : {
      fetchWeather (e) {
        if(e.key == "Enter") {
          fetch(`${this.url_base}weather?q=${this.query}&APPID=${this.api_key}`)
            .then(res => {
              return res.json();
            }).then(this.setResults);
        }
      },
      setResults(results) {
        this.weather = results;
      },
      dateBuilder() {
        let d = new Date();
        let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
        let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

        let day = days[d.getDay()];
        let date = d.getDate();
        let month = months[d.getMonth()];
        let year = d.getFullYear();

        return `${day} ${date} ${month} ${year}`;

      }
  }
}
</script>

<style>
* {
  margin:0;
  padding:0;
  box-sizing:border-box;
}
body {
  font-family : 'montserrat', sans-serif;
}

#app {
  background-image : url('./assets/cold-bg.jpg');
  background-size:cover;
  background-position:bottom;
  transition: 0.4s;
}
#app.warm {
  background-image : url('./assets/warm-bg.jpg');
}

main {
  min-height:100vh;
  padding:25px;
  background-image : linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0,0,0,0.75));
}
.search-box {
  width:100%;
  margin-bottom:30px;
}
.search-box .search-bar {
  display:block;
  width:100%;
  padding:15px;
  color:#313131;
  font-size:20px;
  appearance : none;
  border:none;
  outline:none;
  background:none;
  background-color:rgba(255,255,255, 0.5);
  border-radius:0px 16px 0px 16px;
  transition: 0.4s;
}
.search-box .search-bar:focus {
  background-color:rgba(255,255,255,255,0.75);
  box-shadow : 0px 0px 16px rgba(0,0,0,0.25);
  border-radius:16px 0 16px 0;
}
.location-box .location {
  color : #fff;
  font-size:32px;
  font-weight:500;
  text-align:Center;
  text-shadow : 1px 3px rgba(0,0,0,0.25);
}
.location-box .date {
  color : #fff;
  font-size:20px;
  font-weight:300;
  font-style : italic;
  text-align:Center;
}
.weather-box {
  text-align:Center;
}
.weather-box  .temp {
  display:inline-block;
  padding:15px 25px 0;
  color:#fff;
  font-size:102px;
  font-weight:900;
  text-shadow : 3px 6px rgba(0,0,0,0.25);
  background-color :rgba(255,255,255,0.25);
  border-radius : 16px;
  margin: 30px 0;
  box-shadow : 3px 6px rgba(0,0,0,0.25);
}
.weather-box .weather{
  color:#fff;
  font-size:48px;
  font-weight:700;
  font-style:italic;
  text-shadow : 3px 6px rgba(0,0,0,0.25);
}
</style>
