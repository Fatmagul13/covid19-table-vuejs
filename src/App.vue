<template>
  <div class="container">
    <center><span id="title">Coronavirus Disease (COVID-19) Table</span></center>
    <div id="selectDiv" class="form-group">
      <select
        v-model="selectedCountry"
        class="form-control"
        v-if="countries"
        @change="getDataByCountry"
      >
        <option value="">Global</option>
        <option
          v-for="country in countries"
          :value="country.Slug"
          :key="country.Slug"
        >
          {{ country.Country }}
        </option>
      </select>
    </div>
    <div v-if="data">
      <div class="row justify-content-around">
        <div class="col-md-4">
          <div class="stats-item btn-info">
            <span class="item-title">Confirmed</span>
            <span class="item-count">{{ confirmed | numberFormat }}</span>
          </div>
        </div>
        <div class="col-md-4">
          <div class="stats-item btn-danger">
            <span class="item-title">Deaths</span>
            <span class="item-count">{{ deaths | numberFormat }}</span>
          </div>
        </div>
        <div class="col-md-4">
          <div class="stats-item btn-success">
            <span class="item-title">Recovered</span>
            <span class="item-count">{{ recovered | numberFormat }}</span>
          </div>
        </div>
      </div>
      <hr />
      <component :is="tableType" :data="tableData"></component>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import CountriesTable from "./components/CountriesTable";
import CountryTable from "./components/CountryTable";
export default {
  components: {
    CountriesTable,
    CountryTable,
  },
  data() {
    return {
      countries: {},
      selectedCountry: "",
      data: {},
      countryData: {},
      confirmed: null,
      deaths: null,
      recovered: null,
    };
  },
  computed: {
    tableType() {
      return this.selectedCountry === "" ? "CountriesTable" : "CountryTable";
    },
    tableData() {
      return this.selectedCountry !== "" && this.countryData
        ? this.countryData
        : this.data;
    },
  },
  methods: {
    getCountries() {
      return axios
        .get("https://api.covid19api.com/countries")
        .then((response) => {
          this.countries = response.data;
        });
    },
    getSummaryData() {
      return axios
        .get("https://api.covid19api.com/summary")
        .then((response) => {
          const data = response.data;
          this.data = data.Countries;
          this.confirmed = data.Global.TotalConfirmed;
          this.deaths = data.Global.TotalDeaths;
          this.recovered = data.Global.TotalRecovered;
        });
    },
    getDataByCountry() {
      let country = this.data.filter((country) => {
        return country.Slug == this.selectedCountry;
      });
      country = country[0];
      this.confirmed = country.TotalConfirmed;
      this.deaths = country.TotalDeaths;
      this.recovered = country.TotalRecovered;
      axios
        .get(
          "https://api.covid19api.com/dayone/country/" + this.selectedCountry
        )
        .then((response) => {
          this.countryData = response.data;
        });
    },
  },
  mounted() {
    this.getCountries();
    this.getSummaryData();
  },
};
</script>

<style>

body {
  background: linear-gradient(to bottom left, #d7d3ac, #ffffff);
}
table {
  background: #fff;
}
#title{
  font-weight: bold;
  font-size: 20px;
}
#selectDiv{
  margin-top: 20px ;
}
.container {
  margin-top: 40px;
}
.stats-item {
  padding: 20px;
  text-align: center;
  margin-bottom: 20px;
}
.item-title {
  display: block;
  color: #fff;
  font-size: 17px;
}
.item-count {
  font-weight: bold;
  font-size: 30px;
}
</style>