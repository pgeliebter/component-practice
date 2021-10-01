<template>
  <div class="home">
    <div>{{ queryParamsObject }}</div>
    <!-- <div>{{ setUpCall() }}</div> -->
    <div>{{ nomicsData }}</div>
    <button v-on:click="spitKey()">spit</button>
    <button v-on:click="nomicsGet()">Get Data</button>
    <!-- <img alt="Vue logo" src="../assets/logo.png" /> -->
    <HelloWorld msg="Welcome to Your Vue.js App" />
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue";

export default {
  name: "Home",
  components: {
    HelloWorld,
  },
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
    spitKey: function () {
      console.log(process.env.VUE_APP_NOMICS_KEY);
    },
    setUpCall: async function (unencodedURL = "", queryParams = "") {
      // Default options are marked with *
      const encodedURL = unencodedURL + "?" + queryParams;
      console.log(encodedURL, unencodedURL, queryParams, process.env.VUE_APP_NOMICS_KEY);
      const response = await fetch(encodedURL, {
        method: "Get", // *GET, POST, PUT, DELETE, etc.
        mode: "no-cors", // no-cors, *cors, same-origin
        // credentials: 'same-origin', // include, *same-origin, omit
        headers: {
          "Content-Type": "application/json",
          // Authorization: `Bearer ${process.env.VUE_APP_NOMICS_KEY}`,

          // 'Content-Type': 'application/x-www-form-urlencoded',
        },
        // redirect: "follow", // manual, *follow, error
        // referrerPolicy: "no-referrer", // no-referrer, *no-referrer-when-downgrade, origin, origin-when-cross-origin, same-origin, strict-origin, strict-origin-when-cross-origin, unsafe-url
      });
      return response.json(); // parses JSON response into native JavaScript objects
    },
    nomicsGet: function () {
      async function getData(unencodedURL = "", queryParams = "") {
        // Default options are marked with *
        const encodedURL = unencodedURL + "?" + queryParams;
        console.log(encodedURL, unencodedURL, queryParams, process.env.VUE_APP_NOMICS_KEY);
        const response = await fetch(encodedURL, {
          method: "Get", // *GET, POST, PUT, DELETE, etc.
          mode: "no-cors", // no-cors, *cors, same-origin
          // credentials: 'same-origin', // include, *same-origin, omit
          headers: {
            "Content-Type": "application/json",

            // 'Content-Type': 'application/x-www-form-urlencoded',
          },
          // redirect: "follow", // manual, *follow, error
          // referrerPolicy: "no-referrer", // no-referrer, *no-referrer-when-downgrade, origin, origin-when-cross-origin, same-origin, strict-origin, strict-origin-when-cross-origin, unsafe-url
        });
        return response.json(); // parses JSON response into native JavaScript objects
      }
      const queryParams = Object.keys(this.queryParamsObject)
        .map((key) => `${encodeURIComponent(key)}=${encodeURIComponent(this.queryParamsObject[key])}`)
        .join("&");
      getData(`https://api.nomics.com/v1/currencies/sparkline`, queryParams).then((data) => {
        console.log(data); // JSON data parsed by `data.json()` call
        this.nomicsData = data;
      });
    },
  },
};
</script>
