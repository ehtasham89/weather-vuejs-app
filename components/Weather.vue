<template>
    <div class="weather">
        <h1 v-if="city_details && consolidated" v-text="city_details.title" class="text-center"></h1>
        <div class="card mb-5" v-if="city_details" :style="icon" @click="redirectCity(city_details.woeid)"
             :class="{'text-white bg-primary mb-5':consolidated}" style="cursor: pointer;">
            <div class="card-body">
                <h5 class="card-title" v-text="city_details.title" v-if="!consolidated"></h5>
                <h3 class="card-title text-center" v-if="consolidated">Today</h3>
                <div class="card_subtitle">
                    <h6 v-text="`Temperature : ${Math.round(city_details.consolidated_weather[0].the_temp)} °C`"></h6>
                    <span v-text="city_details.consolidated_weather[0].weather_state_name"></span>
                </div>
                <div class="card-text">
                    <div class="row">
                        <div class="col-md-6">
                                <span class="lead"
                                      v-text="`Max : ${Math.round(city_details.consolidated_weather[0].max_temp)} °C - Min : ${Math.round(city_details.consolidated_weather[0].min_temp)} °C`"></span>

                        </div>
                        <div class="col-md-6">
                            <h3 class="text-danger text-center" v-if="!consolidated">Today</h3>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!--Consolidated Weather-->
        <div v-if="consolidated" v-for="day in consolidatedWeather">
            <div class="card mb-2" :style="getIcon(day.weather_state_abbr)">
                <div class="card-body">
                    <h5 class="card-title" v-text="getDate(day.applicable_date)"></h5>
                    <div class="card_subtitle">
                        <h6 class="text-muted"
                            v-text="`Temperature : ${Math.round(day.the_temp)} °C`"></h6>
                        <span v-text="day.weather_state_name" class="text-info"></span>
                    </div>
                    <div class="card-text">
                        <div class="row">
                            <div class="col-md-6">
                                <span class="lead"
                                      v-text="`Max : ${Math.round(day.max_temp)} °C`"></span>
                                -
                                <span class="lead"
                                      v-text="`Max : ${Math.round(day.min_temp)} °C`"> </span>

                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!--Not Found Exception-->
        <h2 v-if="notFound" class="text-center mt-3"
            v-text="`No results were found for : (${city}). Try changing the keyword!`"></h2>
    </div>
</template>

<script>
    import Router from 'vue-router';
  export default {
    props: ['city', 'woeid', 'consolidated'],
    data() {
      return {
        city_details: false,
        icon_name: null,
        loading: true,
        consolidatedWeather: false,
        notFound: false
      };
    },
    computed: {
      icon() {
        return `background: url("https://www.metaweather.com/static/img/weather/png/${this.icon_name}.png") no-repeat;background-position-x: 100%;background-position-y: 40%;background-size: 14%;`;
      }
    },
    watch: {
      city_details: function (val){
        if (val) {
          this.$emit('notLoading');
        }
      }
    },
    created() {
      if (this.city) this.getWoeid(this.city);
      else if (this.woeid) this.getCityDetails(this.woeid);
      
      document.body.addEventListener('keyup', (e) => {
        (e.keyCode === 72 ) ? this.$router.push({ name: 'Home' }) : ''
      });
    },
    
    methods: {
      async getCityDetails(woeid) {
        const self = this;
        await this.$axios.$get(`http://localhost/fs-code-test/weather-vuejs-app/weather.php?command=location&woeid=${woeid}`)
            .then((response) => {
              let arr = response.consolidated_weather;
              self.consolidatedWeather = arr.splice(1, arr.length);
              self.city_details = response;
              self.icon_name = response.consolidated_weather[0].weather_state_abbr;
            });
      },
      async getWoeid(city) {
        const self = this;
        await this.$axios.$get(`http://localhost/fs-code-test/weather-vuejs-app/weather.php?command=search&keyword=${city}`)
            .then((response) => {
              if (response.length > 0) {
                self.getCityDetails(response[0]['woeid']);
              }
              else {
                self.notFound = true;
              }
            });
      },
      redirectCity(woeid) {
        this.$router.push(`/weather/${woeid}`)
      },
      getIcon(iconName) {
        return `background: url(https://www.metaweather.com/static/img/weather/png/${iconName}.png) no-repeat;background-position-x:100%;background-position-y: 40%;background-size: 14%;`
      },
      getDate(date) {
        let d = new Date(date).toString().replace(/\S+\s(\S+)\s(\d+)\s(\d+)\s.*/,'$1 $2, $3');
        
        return d;
      }
    },
  }
</script>
<style>
.weather {
  animation: 1s appear;
  margin: auto;
}

@keyframes appear {
  0% {
    opacity: 0;
  }
}
</style>
