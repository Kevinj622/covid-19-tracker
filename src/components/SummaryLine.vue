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

    <label for='placeholder'> Please select a country: </label>
    <div>
    <select v-model="selectCountry" id='placeholder'>
      <option
        v-for="option in dropDownList"
        v-bind:value="option.Slug"
        v-bind:key="option.Country"
      >{{option.Country}}</option>
    </select>
    </div>


    <ul v-for="country in findDataByCountry" v-bind:key="country">
      <li>{{country.Country}}</li>
      <li>Deaths: {{country.NewDeaths}}</li>
    </ul>


  <line-chart v-if="loaded" :chartdata ="chartdata" :options="options"> </line-chart>

  </div>
</template>

<script>
import LineChart from "./LineChart.vue";


export default {
  name: "Summary",
  components: {
      
      LineChart
    
    },
    
          
  data() {
    return {

      loaded: false,
      chartdata: null,
      options: {},
      
      //gets the select country
      selectCountry: "",
      dropDownList: [],
      
      author: {
        name: "Kevin McHugh",
        company: "Tech Elevator"
      },

      //all the cases
      summaryData: {},

      //All the countries
      countries: [],

      //countryData: [],

      countryLine: [],


    

    };
  },

  methods: {
    

    //sorts by an object property passed in
    dynamicSort(property) {
      let sortOrder = 1;
      if (property [0] === "-") {
        sortOrder = -1;
        property = property.substr(1);
      }
      return function(a, b) {
        if (sortOrder == -1) {
          return b[property].localeCompare(a[property]);
        } else {
          return a[property].localeCompare(b[property]);
        }
      }
    },

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

    loadData: function(slug) {
       
    this.loaded = false;


      fetch(`${process.env.VUE_APP_REMOTE_API}/dayone/country/${slug}`)
      .then(response => {
        return response.json();
      })
      .then(countries => {
      this.countryLine = countries.map(country => {
          return {
            Confirmed : country.Confirmed,
            Date : country.Date
          }
      });
      
      
      
  
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
    console.log(this.countryLine);
      })
  }

  },
  computed : {
    
    //gets the specific country chosen from the drop down menu
    findDataByCountry() {
      
    const country = this.dropDownList.find(country => {
     return country.Slug === this.selectCountry;
    })
    if (!country) {
      return [];
    }
    const countrySummaryData = this.countries.find(countryFinal => {
      return country.Country === countryFinal.Country;
    })

   

     this.loadData(this.selectCountry);
    
    return [countrySummaryData];
    }
     

  },
  //assigns the global section to summaryData
  //assigns the countries section to countries array
  created() {
    fetch(`${process.env.VUE_APP_REMOTE_API}/summary`)
      .then(response => {
        return response.json();
      })
      .then(summaryData => {
        this.summaryData = summaryData.Global;
        this.countries = summaryData.Countries;
       
        
      });

    //gets all the countries and adds them to the drop down list array
    fetch(`${process.env.VUE_APP_REMOTE_API}/countries`)
      .then(response => {
        return response.json();
      })
      .then(countries => {
        this.dropDownList = countries;
        this.dropDownList.sort(this.dynamicSort('Country'));
      
      });
  },

  watch: {

    selectCountry() {
      console.log("the country has changed");
    }

  }

  
};
</script>

<style>
</style>