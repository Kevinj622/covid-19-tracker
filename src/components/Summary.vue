<template>
  <div>

          <h2>Summary by</h2>

      <p v-for="name in author" v-bind:key="name">{{name}}</p>

   <ul>
      <li>New Confirmed Cases: {{numberFormatter(summaryData.NewConfirmed)}}</li>
      <li>Total Confirmed Cases: {{numberFormatter(summaryData.TotalConfirmed)}}</li>
      <li>New Deaths: {{numberFormatter(summaryData.NewDeaths)}}</li>
      <li>Total Deaths: {{numberFormatter(summaryData.TotalDeaths)}}</li>
      <li>New Recovered: {{numberFormatter(summaryData.NewRecovered)}}</li>
      <li>Total Recovered: {{numberFormatter(summaryData.TotalRecovered)}}</li>
    </ul>


</div>
</template>

<script>

export default {

    name: "Summary",

    data() {
        return {

           
        summaryData: {},

        author: {
        name: "Kevin McHugh",
        company: "Tech Elevator"
         },
        }
    }, 

    methods: {

     numberFormatter(x) {
    return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
    }
    },

    created() {
        fetch(`${process.env.VUE_APP_REMOTE_API}/summary`)
        .then(response => {
            return response.json();
        })
        .then(countries => {
            this.summaryData = countries.Global;
        })
    }


}
</script>

<style>

</style>