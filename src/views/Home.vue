<template>
  <main v-if="!loading">
    <DataTitle :text="title" :dataDate="dataDate" />
    <DataBoxes :stats="stats" />
    <CountrySelect @get-country="getCountryData" :countries="countries" />
    <button @click="clearCountryData" v-if="stats.Country" class="bg-green-700 text-white rounded p-3 mt-10 focus:outline-none hover:bg-green-600">
      Clear Country
    </button>
  </main>
  <main class="flex flex-col align-center justify-center text-center" v-else>
    <div class="text-gray-500 text-3xl mt-10 mb-6">
      Fetching Data
    </div>
    <img :src="loadingImage" class="w-24 m-auto" />
  </main>
</template>

<script>
import axios from "../axios-auth";
import DataTitle from "../components/DataTitle";
import DataBoxes from "../components/DataBoxes";
import CountrySelect from "../components/CountrySelect";

export default {
  name: "Home",
  components: {
    DataTitle,
    DataBoxes,
    CountrySelect,
  },
  created() {
    this.fetchData();
  },
  data() {
    return {
      loading: true,
      title: "Global",
      dataDate: null,
      stats: {},
      countries: [],
      loadingImage: require("../assets/hourglass.gif"),
    };
  },
  methods: {
    fetchData() {
      axios
        .get("/summary")
        .then((response) => {
          console.log(response);
          this.loading = false;
          this.dataDate = response.data.Date;
          this.stats = response.data.Global;
          this.countries = response.data.Countries;
          this.title = 'Global'
        })
        .catch((err) => {
          console.log(err);
        });
    },
    getCountryData(country) {
      this.stats = country;
      this.title = country.Country;
    },
    clearCountryData(){
      this.loading = true;
      this.fetchData();
    }
  },
};
</script>
