<template>
   <div class="weather-container">
        <!-- <p>citycode: {{ citycode }}</p>
        <p>维度：{{ lat }}</p>
        <p>经度：{{ lng }}</p> -->
        <weather-detail :weatherdata="weatherdata" :lifedata="lifedata" :weatherdate="weatherdate" v-if="flag"></weather-detail>
    </div>
</template>

<script>
import { mapState } from "vuex";
import axios from "axios";
import WeatherDetail from "./ChatBotWeatherDetail";

export default {
  name: "weather",
  props: ["destination", "date"],
  components: { WeatherDetail },
  data() {
    return {
      weatherdata: {},
      lifedata: {},
      weatherdate: {},
      flag: false
    };
  },
  mounted() {
    this.getWeather();
    this.getLife();
    // this.getWeatherdate();
  },

  methods: {
    getWeather() {
      axios
        .get(
          "https://free-api.heweather.com/s6/weather/forecast?&location=" +
            this.destination +
            "&key=1acdfb31bc4e465a96b428d43e5367e9"
        )
        .then(res => {
          var res = res.data;
          // this.$store.state.weatherdata = res;
          this.weatherdata = res;
          console.log(this.weatherdata);
          this.flag = true;
          var weatherdate_lst = this.date[0].split(" ");
          var weatherindex_lst = this.date[1].split(" ");
          var weatherdate = weatherdate_lst[0];
          // console.log(weatherdate);
          var weatherindex = parseInt(weatherindex_lst[0]);
          var arr = this.weatherdata.HeWeather6[0].daily_forecast;
          // console.log(arr);
          for (var j = 0; j < arr.length; j++) {
            // console.log(arr[j].date);
            if (arr[j].date === weatherdate) {
              // console.log(arr[j].date);
              var new_arr = arr.slice(j, j + weatherindex);
              // console.log(cnew_arr);
              this.weatherdate = new_arr;
              console.log(this.weatherdate);
            }
          }
        });
    },
    getLife() {
      axios
        .get(
          "https://free-api.heweather.com/s6/weather/lifestyle?parameters&location=" +
            this.destination +
            "&key=1acdfb31bc4e465a96b428d43e5367e9"
        )
        .then(res => {
          var res = res.data;
          this.lifedata = res;
          console.log(this.lifedata);
        });
    },
    getWeatherdate() {
      var weatherdate_lst = this.date[0].split(" ");
      var weatherindex_lst = this.date[1].split(" ");
      var weatherdate = weatherdate_lst[0];
      console.log(weatherdate);
      var weatherindex = parseInt(weatherindex_lst[0]);
      var arr = weath.HeWeather6[0].daily_forecast;
      for (j = 0; j < arr.length; j++) {
        if (arr[j] === weatherdate) {
          var new_arr = arr[(j, j + weatherindex)];
          this.weatherdate = new_arr;
        }
      }
    }
  }
  //   computed: {
  //       // Getting Vuex State from store/modules/weather
  //     ...mapState({
  //         citycode: state => state.weather.citycode,
  //     }),
  //   },
  //   created () {
  //       console.log('longtitude!!!!!!!!!!')
  //       console.log(this.lng)
  //       console.log('latitude!!!!!!!!!!')
  //       console.log(this.lat)
  //     // Getting longitude and latitude
  //     // const lng = this.lng
  //     // const lat = this.lat

  //     this.$store.dispatch({
  //         type: 'getCityCode',
  //         lng: this.lng,
  //         lat: this.lat
  //     }).then(res => {
  //         console.log('0')
  //         console.log(res)
  //     });
  //   }
};
</script>

<style lang="scss" scoped>
.weather-container {
  height: 200px;
  display: inline-block;
}
</style>