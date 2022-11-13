<template>
    <div id="weather-container">
      <div class="jacket-display">{{ answer }}</div>
      <div class="precip-display">{{ alsoAnswer }}</div>
    </div>
</template>

<script>
export default {
  name: 'WeatherComp',
  data() {
    return {
      locInfo: null,
      weatherInfo: null,
      answer: null,
      alsoAnswer: null
    }
  },
  props: {
  },
  methods: {
    recordPosition: async function(position) {
      this.locInfo = position.coords;
      const url = 'https://www.7timer.info/bin/api.pl?lon=' + this.locInfo.longitude + '&lat=' + this.locInfo.latitude + '&product=astro&output=json'
      const response = await fetch(
        url
      );
      await response.json().then((data) => this.weatherInfo = data.dataseries);
    },
    determineTemp: function(temp) {
      if (temp < 0) {
        this.answer = "YES, A HEAVY ONE";
      } else if (temp <= 9) {
        this.answer = "YES, A MEDIUM ONE";
      } else if (temp <= 19) {
        this.answer = "YES, A LIGHT ONE";
      } else if (temp <= 29) {
        this.answer = "NO";
      } else {
        this.answer = "Definitely not and you should probably also just stay inside, its hot."
      }
    },
    determinePrecip: function(precip) {
      if (precip === 'rain') {
        this.alsoAnswer = "Also maybe bring an umbrella";
      } else if (precip === 'snow') {
        this.alsoAnswer = 'Also keep in mind that it\'s snowing';
      } else if (precip === 'none') {
        this.alsoAnswer = 'Also, it\'s dry out';
      }
    }
  },
  watch: {
    weatherInfo() {
      if (!this.weatherInfo[0]) {
        return null;
      } else {
        this.determineTemp(this.weatherInfo[0].temp2m);
        this.determinePrecip(this.weatherInfo[0].prec_type)
      }
    }
  },
  mounted() {
    navigator.geolocation.getCurrentPosition(this.recordPosition);
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.main-div .jacket-display {
  font-size: 45px;
}
.main-div .precip-display {
  font-size: 40px;
}
</style>
