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
        return months[date.getMonth() + 1] + ' ' + (date.getDate() + 1);
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
            Deaths : country.Deaths,
            Recovered: country.Recovered,
            Active: country.Active,
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
        label: 'Confirmed',
        backgroundColor: this.getRandomHex(),
        data: this.countryLine.map(data => data.Confirmed)
        },

        {
        label: 'Deaths',
        backgroundColor: this.getRandomHex(),
        data: this.countryLine.map(data => data.Deaths)
        },
          {
        label: 'Recovered',
        backgroundColor: this.getRandomHex(),
        data: this.countryLine.map(data => data.Recovered)
        },
        {
        label: 'Active',
        backgroundColor: this.getRandomHex(),
        data: this.countryLine.map(data => data.Active)
        },
        
    ]
    },

    this.options = {

      responsive : true,
      maintainAspectRatio: false,
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