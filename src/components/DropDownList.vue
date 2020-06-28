<template>
  <div>
    <div>
    
    <label for='placeholder'> Please select a country: </label>
    <div>
    <select @change="findCountry" v-model="selectedCountry" id='placeholder'> 
        <option v-for="option in dropDownList"
        v-bind:key="option"
        v-bind:value="option.Slug">  
        {{option.Country}}
        </option>
    </select>
    </div>

    </div>
    </div>
</template>

<script>
export default {
name: "DropDownList",

data() {
    return {
        
        selectedCountry : "",
        dropDownList : []
    }
},


methods: {

   findCountry() {
  
        this.$emit("country-changed", this.selectedCountry);
      
    },



    sortCountries(property) {
        let sortOrder = -1;
        if (property[0] === "-") {
            sortOrder = 1;
            property = property.substr(1);
        }
    return function(a, b) {
        if (sortOrder === 1) {
            return b[property].localeCompare(a[property]);
        } else {
            return a[property].localeCompare(b[property]);
        }
    }
        
    }
    },


created() {

    fetch(`${process.env.VUE_APP_REMOTE_API}/countries`)
    .then(response => {
        return response.json();
    })
    .then(countries => {
        this.dropDownList = countries;
        this.dropDownList.sort(this.sortCountries('Country'));
    
    }) 
}
}


</script>

<style>


</style>