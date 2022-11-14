<template>
<div id="app" :class="typeof weather.main !='undefined' && weather.main.temp > 16 ? 'warm' : ''">
  <main>
    <div v-if="weather">
      <div class="searchBox">
      <input 
       type="text"
       class="searchBar" 
       placeholder="Search..."
       v-model="searchValue"
       >
       <button @click="fetchWeather()" class="search-btn">Search</button>
    </div>
    <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
      <div class="locationbox">
        <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
        <div class="date"> {{dateBuilder()}}</div>
      </div>

      <div class="weather-box">
        <div class="temp">{{ Math.round(weather.main.temp) }} °C</div>
        <div class="weather"> {{ weather.weather[0].main}}</div>
      </div>
    </div>
    </div>
    <div v-else>
      <P>City not found</P>
    </div>
    
  </main>
</div>
</template>

<script>

export default {
  name: 'App',
  data(){
    return {
      api_key: "27bcc252742830381afd6856832da01a",
      url_base: "https://api.openweathermap.org/data/2.5/",
      searchValue:'',
      weather:{},
      err:'city not found'
    }
  },
  methods:{
    fetchWeather(){
      fetch(`${this.url_base}weather?q=${this.searchValue}&units=metric&APPID=${this.api_key}`)
      .then(
        res=>{
          return res.json();
        }
      ).then(
        this.setResults
      ).catch(
        (error)=>{
          console.error(error)
          this.weather={};
        }
      )
    },
    setResults(results){
      this.weather=results;
      console.log(this.weather)
    },
    dateBuilder(){
      let d=new Date();
      let months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];
      let days  = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday']
      let day=days[d.getDay()];
      let date=d.getDate();
      let month=months[d.getMonth()];
      let year=d.getFullYear();
      return `${day} ${date} ${month} ${year}`
    }
  }
}
</script>

<style>
  .search-btn{
    border-radius: 10px;
    border: none;
    margin-left: 10px;
    font-size: 20px;
    color: #313131;
    padding: 10px;
  }
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body{
  font-family: 'montseratt';
}
#app{
  background-image:url('./assets/cold.jpg') ;
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
  
}
#app.warm{
  background-image: url('./assets/warm.jpg');
}
main{
  min-height: 100vh;
  background-image: linear-gradient(to bottom,rgba(0,0,0,0.25),rgba(0,0,0,0.75));
}
.searchBox{
  padding: 25px;
   display: flex;
  justify-content: center;

}
.searchBox .searchBar{
  display: flex;
  justify-content: center;
  
  padding: 15px;
  appearance: none;
  font-size: 20px;
  color: #313131;
  border: none;
  outline: none;
  background: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}
.searchBox .searchBar:focus {
  border-radius: 16px 0px 16px 0px;
  background-color: rgba(255, 255, 255, 0.75);
   box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
}
.locationbox .location{
  font-size: 32px;
  text-align: center;
  color: white  ;
  font-weight: 500;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.locationbox .date{
  margin-top: 10px;
  font-size: 20px;
  text-align: center;
  color: white  ;
  font-weight: 200;
  font-style: italic;
}
.weather-box{
  text-align: center;
  padding: 20px;
}
.weather-box .temp {
  display: inline-block;
  color: white;
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 10px;
  font-size: 52px;
  font-weight: 900;
  padding: 20px;
  box-shadow: 3px 6px rgba(0,0,0,0.25);
} 
.weather-box .weather{
   color: white;
   font-size:  42px;
   font-weight: 600;
   margin-top: 10px;
   font-style: italic;
}
</style>
