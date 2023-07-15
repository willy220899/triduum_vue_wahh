<template>
  <div id="app">
    <img style="margin-bottom: 3rem; margin-top: 3rem;" alt="Vue logo" src="./assets/star-wars-logo.png" width="60%" height="100%">
    <HelloWorld/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App', 
  components: {
    HelloWorld
  }
}
</script>

<style>
#app {
  background-color: black;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
