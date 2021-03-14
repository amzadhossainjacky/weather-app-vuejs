<template>
  <div class="container-fluid" id="main" :class="(typeof weather.main !='undefined') &&  weather.main.temp <20 ? 'cold': '' ">
    <div class="over-lay">
    </div>
      <div class="row text-center justify-content-center">
        <div class="col-10 col-sm-8 col-md-8 col-lg-8">
          <input type="text" class="form-control mt-4 in-design" placeholder="Search ..." v-model="query" @keypress="fetchWeather">
        </div>
          <div class="col-12"></div>
          <div class="col-7 col-sm-7 col-md-4 col-lg-4">

            <div class="weather-wrap" v-if="(typeof weather.main !='undefined')">
                  <div class="location-box">
                <h2 class="location">{{weather.name}}, {{weather.sys.country}}</h2>
                <p class="date">{{dateBuilder()}}</p>
              </div>

              <div class="weather-box">
                <h2 class="temp">{{Math.round(weather.main.temp)}}&#176;C</h2>
                <p class="status"> {{weather.weather[0].main}}</p>
              </div>
            </div>
          
        </div>
      </div>
  </div>
</template>


<script>
export default {
  data() {
    return {
      api_key: 'f1440d6c0da80f4026a275363ac1f5e3',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}

    }
  },
  methods: {
    fetchWeather(e){
      if(e.key == "Enter"){
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`).then(res => {
        return res.json();
      }).then(this.setResults);
      }
    },
    
    setResults(results){
     this.weather = results
     console.log(results)
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
    }
  }
  
}
</script>


<style >

  #main{
    background: url('./assets/weather/sunny.jpg');
    background-size: cover;
    background-repeat: no-repeat;
    background-position: center;
    min-height: 100vh;
    background-color: #000 !important;
    position: relative;
    font-family: 'Recursive', sans-serif;
  }

  .over-lay{
    position: absolute;
    background: #000;
    left: 0;
    top: 0;
    height: 100%;
    width: 100%;
    opacity: 0.4;
  }

  .in-design{
    border-radius: 5px 20px 5px 20px;
  }


  .form-control:focus {
    background-color: rgb(243, 241, 241);
    outline: 0px;
    border: 0px solid #fff;
    box-shadow: 5px 10px 8px rgba(0, 0, 0, 0.178);
    transition: all 0.4s ease;
  }

  .location-box{
    color: rgb(219, 219, 219);
  }
  .location-box h2{
    margin: 15px 0px 5px 0px;
    letter-spacing: .025rem;
  }
  
  .location-box p{ 
    font-size: 20px;
    letter-spacing: .025rem;
    font-style: italic;
  }

  .weather-box{
    color: #fff;
  }
  .weather-box .temp{
    background-color: rgba(255, 255, 255, 0.178);
    padding: 25px;
    border-radius: 15px;
    font-size: 80px;

  }
  .weather-box .status{
      font-size: 40px;
      letter-spacing: .025rem;
      font-weight: 500;
      color: rgb(219, 219, 219);
      font-style: italic;
    }

  #main.cold{
    background: url('./assets/weather/cold2.jpg');
    background-size: cover;
    background-repeat: no-repeat;
    background-position: center;
  }
  
  
</style>