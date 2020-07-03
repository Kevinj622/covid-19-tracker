<template>
  <div id="app">
  <header> 

    

  </header>
  <h1> COVID-19 Tracker </h1>
  <Summary> </Summary>
  <DropDownList v-on:country-changed="updateSelectedCountry"> </DropDownList>

  <div class="container"> 
  <SummaryLine class="box" 
  :selectedCountry="selectedCountry" 
  :selectedStatus="selectedStatus"> 
  </SummaryLine>
  <SummaryPie class="box" :selectedCountry="selectedCountry"> </SummaryPie>
  </div>
  
  </div>
</template>

<script>

import Summary from './components/Summary.vue'
import SummaryLine from './components/SummaryLine.vue'
import SummaryPie from './components/SummaryPie.vue'
import DropDownList from './components/DropDownList.vue'

export default {
  name: 'App',
  components: {
    SummaryLine,
    SummaryPie,
    DropDownList,
    Summary
  },
  data() {
    return {

      selectedCountry : "",
      selectedStatus : ""
    }
  },


  methods: {

    updateSelectedCountry(country) {

      this.selectedCountry = country;
     
    }

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
