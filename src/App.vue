<template>

  <div id="app"  :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''" >
    <hello-world @setWeather="setWeather" @setRandom="setRandom" :weather=weather :random=random /> 
  </div>
</template>

<script>
import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  },
  data(){
    return{
      weather: {},
      random: "faith"
    }
  },
  methods: {
    setWeather(results){
      this.weather = Object.assign({}, this.weather, results);
      // console.log("emitted: ", results);
    },
    setRandom(name){
      this.random = name;
    }
  }
}
</script>

<style>
*{
  box-sizing: border-box;
  margin: 0;
  padding: 0;

}

body{
  font-family: 'monteserrat', sans-serif;
  background-color: rgb(223, 246, 252);
}

#app{
  text-align: center;
  min-height: 100vh;
  max-width: 500px;
  background-image: url('./assets/images/cold-bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
  margin: 0 auto;
  overflow: hidden;
}

#app.warm{
  background-image: url('./assets/images/warm-bg.jpg');
}
</style>
