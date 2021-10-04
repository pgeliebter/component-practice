<template>
  <div class="home">
    <div>{{ queryParamsObject }}</div>
    <!-- <div>{{ setUpCall() }}</div> -->
    <div>{{ nomicsData }}</div>
    <button v-on:click="spitKey()">spit</button>
    <button v-on:click="nomicsGet()">Get Data</button>
    <button v-on:click="nomicsGetWithService()">Get Data with service</button>

    <!-- <img alt="Vue logo" src="../assets/logo.png" /> -->
  </div>
</template>

<script>
import HttpService from "@/services/HttpService.vue";

import axios from "axios";
export default {
  data: function () {
    return {
      queryParamsObject: {
        key: process.env.VUE_APP_NOMICS_KEY,
        start: "2018-04-14T00:00:00Z",
        ids: "BTC",
        end: "2018-04-14T00:00:00Z",
      },
      nomicsData: {},
      key: "",
    };
  },
  methods: {
    nomicsGetWithService: function () {
      HttpService.get(
        `https://api.nomics.com/v1/currencies/sparkline`,
        {
          params: this.queryParamsObject,
        },
        (status, data) => console.log(status, data)
      );
    },
    nomicsGet: function () {
      axios
        .get(`https://api.nomics.com/v1/currencies/sparkline`, {
          params: this.queryParamsObject,
        })
        .then((response) => {
          this.nomicsData = response.data;
          console.log(this.nomicsData);
        })
        // .then(console.log(this.nomicsData))
        .catch((error) => console.log(error));
    },
  },
};
</script>
