<template>
  <div>


  <p> {{countryLine[0]['Country']}} </p>
  <line-chart v-if="loaded" :chartdata ="chartdata" :options="options"> </line-chart>

  </div>
</template>

<script>
import LineChart from "./LineChart.vue";


export default {
  name: "SummaryLine",
  components: {
      
      LineChart
  
    },
  
  props : {

    selectedCountry: String

  },
    
  data() {
    return {

      loaded: false,
      chartdata: null,
      options: {},
      
      countryLine: [],
    };
  },

  methods: {
  
    // getCountryLabel : function(countryData) {

    //   return `${countryData.Country}: (${countryData.TotalConfirmed})`

    // },
    getRandomHex: function() {
      return "#" + Math.floor(Math.random() * 16777215).toString(16);
    },

    getDateLabel: function(timeStamp) {
        const date = new Date(timeStamp);
        const months = {
          "1" : "Jan",
          "2" : "Feb",
          "3" : "Mar",
          "4" : "Apr",
          "5" : "May",
          "6" : "Jun",
          "7" : "Jul",
          "8" : "Aug",
          "9" : "Sep",
          "10" : "Oct",
          "11" : "Nov",
          "12" : "Dec"
        };
        return months[date.getMonth() + 1] + (date.getDate() + 1);
    },

        getSelectedCountry() {

      fetch(`${process.env.VUE_APP_REMOTE_API}/dayone/country/${this.selectedCountry}`)
      .then(response => {
        return response.json();
      })
      .then(countries => {
      this.countryLine = countries.map(country => {
          return {
            Country : country.Country,
            Confirmed : country.Confirmed,
            Date : country.Date
          }
      })
      this.loadData();

      })

          
        },

    loadData(){
       
    this.loaded = false;

    this.chartdata = {
      labels: this.countryLine.map(sd => this.getDateLabel(sd.Date)),

      datasets: [
        {
        label: "Total Confirmed",
        backgroundColor: this.getRandomHex(),
        data: this.countryLine.map(data => data.Confirmed)
        },
    ]
    }
  this.loaded= true;
   
  }

  },
  
  created() {

    this.getSelectedCountry();

 
  },

  watch: {

    selectedCountry() {
        this.loaded = false;
        this.getSelectedCountry();
       
    }

  }

  
};
</script>

<style>
</style>