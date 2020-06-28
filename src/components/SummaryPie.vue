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

            filteredCountry: {}
        }
    },

    methods: {
        getRandomHex: function() {
      return "#" + Math.floor(Math.random() * 16777215).toString(16);
    },

        getSelectedCountry() {

            fetch(`${process.env.VUE_APP_REMOTE_API}/total/country/${this.selectedCountry}`)
            .then(response => {
                return response.json();
            })
            .then(countries => {
                this.filteredCountry = countries[countries.length - 1];
                console.log(this.filteredCountry);
                this.loadData();
            })

        },

        loadData() {
            this.loaded = false;
        
            this.data = {
                datasets: [{
                backgroundColor : [this.getRandomHex(), 
                                    this.getRandomHex(), 
                                    this.getRandomHex(), 
                                    this.getRandomHex()],
                data: [this.filteredCountry.Confirmed,
                       this.filteredCountry.Active,
                       this.filteredCountry.Deaths,
                       this.filteredCountry.Recovered
                       ]
                }],

    // These labels appear in the legend and in the tooltips when hovering different arcs
            labels: [
                    'Confirmed',
                    'Active',
                    'Deaths',
                    'Recovered'
                    ]
                };
        

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