<template>
  <div class="hello">
    <main>
      <div class="search-box">
        <input 
          type="text" 
          class="search-bar" 
          placeholder="Search..." 
          v-model="query"
        />
        <button @click="fetchWeather" class="search-btn">Get Weather</button>
      </div>
      <div v-if="loading === true" class="loader">
        <Loader />
      </div>
      <div v-if="error === true" class="error-text">
        An error occured
      </div>
      <div class="weather-wrap" v-if="weather.name !== undefined && loading === false">
        <div class="location-box">
            <weather-icons :weather=weather />
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
   
  </div>
</template>

<script>
import WeatherIcons from '@/components/WeatherIcons.vue';
import Loader from '@/components/Loader.vue';
import axios from 'axios';
export default {
  name: 'HelloWorld',
  components: {
    WeatherIcons,
    Loader
  },
    
  props: ['weather'],
  data(){
    return{
      api_key: 'e46982639ac21d94fcc3aa807b37727d',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      LocalRandom: '',
      loading: false,
      error: false
    }
  },
  mounted(){
    this.LocalRandom = this.random;
  },
  methods: {
    fetchWeather(){
      const empty = {
        base: undefined,
        clouds: undefined,
        cod: undefined,
        coord:undefined,
        dt:undefined,
        id:undefined,
        main:undefined,
        name:undefined,
        sys:undefined,
        timezone:undefined,
        visibility:undefined,
        weather:undefined,
        wind:undefined
      }

      this.loading = true;
      this.error = false;
      const self = this
      axios.get(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`, {headers: {'Content-Type': 'application/json'}})
      .then(res => {this.setResults(self, res.data); this.loading = false;}, (err) => {this.error = true; this.loading = false; this.setResults(self, empty); Promise.reject(err);});
      
    },
    setResults(self, results){
      self.$emit('setWeather', results);
    },
    dateBuilder(){
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day}, ${date} ${month} ${year}`;
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
main{
  width: 100%;
  min-height: 100vh;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}

.search-box{
  width: 100%;
  margin-bottom: 30px;
  padding: 30px;
  display: flex;
  justify-content: space-between;
}

.search-btn{
  padding: 0 20px;
  background-color: rgb(69, 93, 226);
  border: none;
  color: white;
  font-size: 16px;
  margin-left: 20px;
  border-radius: 20px;
}

.search-btn:hover{
  opacity: 0.6;
}

.search-box .search-bar{
  display: block;
  width: 65%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  border: none;
  outline: none;
  background: none;
  box-shadow: 0 0 8px rgba(255, 255, 255, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0 16px 0 16px;
}

.search-box .search-bar:focus{
  box-shadow: 0 0 16px rgba(255, 255, 255, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0;
}

.search-box .search-bar:focus{
  background-color: rgba(255, 255, 255, 0.75);
}

.loader{
  width: 70px;
  height: 70px;
  text-align: center;
  margin: auto;
  display: flex;
  justify-content: center;
  margin-top: 120px;
}

.location-box .location{
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);

}

.location-box .date{
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}
.weather-obj{
  font-size: 50px;
}
.weather-box{
  text-align: center;
}

.weather-box .temp{
  display: inline-block;
  padding: 25px 25px;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  color: #fff;
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 65px 0;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather, .error-text{
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

 .error-text{
   color: rgba(253, 39, 39, 0.719);
   margin-top: 100px;
 }
</style>
