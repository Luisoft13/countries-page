<template>
  <b-container fluid class="pl-0 pr-0">
    <div class="content_title">
      <h2 class="font-weight-bold">Where in the World?</h2>
    </div>

    <div style="background:#FAFAFA">
      <b-row class="content_header">
        <b-col sm="4" cols="12" class="text-align-left">
          <b-form-input v-model="countryName" placeholder="Search you country"></b-form-input>
        </b-col>
        <b-col sm="3" cols="12">
          <b-form-select v-model="getCountriesByRegion" placeholder="Filter by Region" :options="options"></b-form-select>
        </b-col>
      </b-row>
      <b-row class="content_body">
        <b-col
          v-for="country in countriesTotal"
          :key="country.alpha2Code"
          lg="4"
          xl="3"
        >
          <b-card :img-src="country.flag" img-top class="mb-5">
            <b-card-body>
              <h3> {{ country.name}}</h3>
              <h5>Population: {{ country.population}}</h5>
              <h5>Región: {{ country.region}}</h5>
              <h5>Caputal: {{ country.capital}}</h5>
            </b-card-body>
          </b-card>
        </b-col>
      </b-row>
    </div>
  </b-container>
</template>

<script>
import axios from "axios";
export default {
  name: 'HelloWorld',
  data() {
    return {
      getCountriesByRegion: "",
      countriesTotal: [],
      countriesTotalOrigin: [],
      countryName: "",
      options: [
          { value: "", text:'Filter by Region'},
          { value: "", text: 'Todos' },
          { value: 'Africa', text: 'Africa' },
          { value: 'Americas', text: 'Americas' },
          { value: 'Europe', text: 'Europe' },
          { value: 'Asia', text: 'Asia' },
          { value: 'Oceania', text: 'Oceania' },


        ]
    }
  },
  created() {
    axios
      .get("https://restcountries.com/v2/all")
      .then((response) => {
        this.countriesTotal = response.data;
        this.countriesTotalOrigin = response.data;
        console.log(response.data)
      })
      .catch((error) =>
        console.log("No se encontró la información", error)
      );
  },
  watch: {
    countryName(newValue) {
      this.countriesTotal = this.countriesTotalOrigin
      if(newValue) {
        if(this.getCountriesByRegion != "") {
          this.countriesTotal = this.countriesTotal.filter(
          (country) => country.region == this.getCountriesByRegion
        )
        }
        this.countriesTotal = this.countriesTotal.filter((country) =>
          country.name.toLowerCase().includes(newValue) ||
          country.name.includes(newValue)
        );
      }
    },
    getCountriesByRegion(newValue) {
      if(newValue) {
        this.countriesTotal = this.countriesTotalOrigin.filter(
          (country) => country.region == newValue
        )
      }
    }
  },
}
</script>
<style scoped>
.content_title {
  padding: 20px 80px 20px 80px;
  -webkit-box-shadow: 3px 3px 5px 6px rgba(0, 0, 0, 0.4);  /* Safari 3-4, iOS 4.0.2 - 4.2, Android 2.3+ */
  -moz-box-shadow: 3px 3px 5px 6px rgba(0, 0, 0, 0.4);  /* Firefox 3.5 - 3.6 */
  box-shadow: 3px 3px 5px 6px rgba(0, 0, 0, 0.4);
}
.content_header {
  padding: 20px 80px 20px 80px;
  justify-content: space-between;
  margin: 0px;
}
.content_body {
  padding: 20px 80px 20px 80px;
  margin:0px;
}
</style>
