<template>
    <div>

    <p> {{filteredCountry['Country']}}</p>

    <pie-chart v-if ="loaded" :data ="data" :options ="options"> </pie-chart>
    
    </div>

</template>

<script>
import PieChart from './PieChart.vue'


export default {
    
    name: "SummaryPie",
        components : {
            PieChart,
       
        }, 
    props : {

        selectedCountry: String
    },
    data() {
        return {
            loaded :false,
            data : null,
            options : null,

            filteredCountry: {},

            stats: []


        }
    },



    methods: {
        getRandomHex: function() {
      return "#" + Math.floor(Math.random() * 16777215).toString(16);
    },
    

        getPercentage(number, array) {
            let total = 0;
            array.forEach(el => {
                total += el;
            })

            return (number / total * 100).toFixed(2);
        },


        getSelectedCountry() {

            fetch(`${process.env.VUE_APP_REMOTE_API}/total/dayone/country/${this.selectedCountry}`)
            .then(response => {
                return response.json();
            })
            .then(countries => {
                this.filteredCountry = countries[countries.length - 1]; 

                let {Active, Deaths, Recovered} = this.filteredCountry;
                this.stats[0] = Active;
                this.stats[1] = Deaths;
                this.stats[2] = Recovered;
                console.log(this.stats);
                this.loadData();
            })

        },

        loadData() {
            this.loaded = false;
        
            this.data = {
                labels: ['Active', 'Deaths', 'Recovered'],
                datasets: [{
                backgroundColor : [
                                    this.getRandomHex(), 
                                    this.getRandomHex(), 
                                    this.getRandomHex()],

                data: [
                       this.getPercentage(this.stats[0], this.stats),
                      this.getPercentage(this.stats[1], this.stats),
                       this.getPercentage(this.stats[2], this.stats),
                       ]
                }],

                };
        
            this.options = {

            responsive : true,
            maintainAspectRatio: false,
        }

        this.loaded = true;
        }

    },
    created () {

            this.getSelectedCountry();
    
    },

    watch:  {

        selectedCountry() {
            this.loaded = false;
           
            this.getSelectedCountry();
      

        }
    }


    }
</script>


<style>

</style>