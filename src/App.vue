<template>
  <div id="app">
    <Swiper/>
  </div>
</template>

<script>
import Swiper from './components/Swiper'

export default {
  name: 'App',
  components: {
    Swiper
  }
}
</script>

<style>
#app {
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  width: 250px;
  height: 320px;
  margin-top: 100px;
}
</style>
