<template>
  <div id="app">
  <h1 class="box"> COVID-19 Tracker </h1>


  <Summary class="box"> </Summary>
  
  
  </div>
</template>

<script>


import Summary from './components/Summary.vue'


export default {
  name: 'App',
  components: {
    Summary
  }
}
</script>

<style>

@import "../node_modules/bulma/css/bulma.css";
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
