<template>
  <div id="app">
  <h1 class="box"> COVID-19 Tracker </h1>


  <SummaryLine class="box"> </SummaryLine>
  
  
  </div>
</template>

<script>


import SummaryLine from './components/SummaryLine.vue'


export default {
  name: 'App',
  components: {
    SummaryLine
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
