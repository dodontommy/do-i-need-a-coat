<template>
  <div class="main-div">
    <WeatherComp/>
  </div>
</template>

<script>
import WeatherComp from './WeatherComp.vue'

export default {
  name: 'MainApp',
  components: {
    WeatherComp
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.main-div {
  text-align: center;
}
</style>
