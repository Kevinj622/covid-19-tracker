<template>
  <div>
    <h2>Summary by</h2>

    <p v-for="name in author" v-bind:key="name">{{name}}</p>

    <ul>
      <li>New Confirmed Cases: {{summaryData.NewConfirmed}}</li>
      <li>Total Confirmed Cases: {{summaryData.TotalConfirmed}}</li>
      <li>New Deaths: {{summaryData.NewDeaths}}</li>
      <li>Total Deaths: {{summaryData.TotalDeaths}}</li>
      <li>New Recovered: {{summaryData.NewRecovered}}</li>
      <li>Total Recovered: {{summaryData.TotalRecovered}}</li>
    </ul>


    <select v-model="selectCountry">
      <option
        v-for="option in dropDownList"
        v-bind:value="option.Country"
        v-bind:key="option.Slug"
      >{{option.Country}}</option>
    </select>


    <div>
      <form @submit.prevent="findDataByCountry">
        <button type="submit">Search for Country</button>
      </form>
    </div>
    <ul v-for="country in findDataByCountry" v-bind:key="country">
      <li>{{country.Country}}</li>
      <li>Deaths: {{country.NewDeaths}}</li>
    </ul>

  <BarChart v-if="loaded" :chartdata ="chartdata" :options="options"> </BarChart>

  </div>
</template>

<script>
import BarChart from "./BarChart.vue";

export default {
  name: "Summary",
  components: {BarChart},
  data() {
    return {

      loaded: false,
      chartdata: null,
      options: {},
      
      selectCountry: "",
      dropDownList: [],
      
      author: {
        name: "Kevin McHugh",
        company: "Tech Elevator"
      },

      summaryData: {},

      countrySummaries: [],

      countryData: []
    };
  },

  methods: {
    getRandomHex: function() {
      return "#" + Math.floor(Math.random() * 16777215).toString(16);
    },

    loadData: function(country) {
       
    this.loaded = false;

 
  
    this.chartdata = {
      labels: ["Total Confirmed"],

      datasets: [{
        barPercentage: 0.5,
        barThickness: 6,
        maxBarThickness: 8,
        minBarLength: 2,
        data: [country.TotalConfirmed]

    }]
    }

  this.loaded= true;
  }
    

  },
  computed : {
    
    //gets the specific country chosen from the drop down menu
    findDataByCountry() {
      
    const country = this.countrySummaries.filter(country => {
     return country.Country === this.selectCountry;
    })
    if (country.length > 0) {

      this.loadData(country[0]);
    }
    return country;
    }
     

  },
  //assigns the global section to summaryData
  //assigns the countries section to countrySummaries
  created() {
    fetch(`${process.env.VUE_APP_REMOTE_API}/summary`)
      .then(response => {
        return response.json();
      })
      .then(summaryData => {
        this.summaryData = summaryData.Global;
        this.countrySummaries = summaryData.Countries;
       
      });

    //gets all the countries and adds them to the drop down list array
    fetch(`${process.env.VUE_APP_REMOTE_API}/countries`)
      .then(response => {
        return response.json();
      })
      .then(countries => {
        this.dropDownList = countries;
      });
  },
  mounted () {

this.loaded = false;

 
  
    this.chartdata = {
      labels: ["Total Confirmed"],

      datasets: [{
        barPercentage: 0.5,
        barThickness: 6,
        maxBarThickness: 8,
        minBarLength: 2,
        data: [10, 20]
    }]
    }
    this.options= {      
      responsive: true,     
      maintainAspectRatio: false    
      }
    

  this.loaded= true;

  }

  
};
</script>

<style>
</style>