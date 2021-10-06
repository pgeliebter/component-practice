<template>
  <div class="home">
    <button v-on:click="coinGeckoGetPrice(getCryptoData)">Get Data</button>

    <div>
      <div>
        <label for="oldDate">Historical Date</label>

        <input v-model="getCryptoData.historicalDate" type="text" id="oldDate" />
      </div>
      <select v-model="selectedCrypto">
        <option v-bind:value="crypto" v-for="crypto in cryptos" v-bind:key="crypto.id">
          {{ crypto.name }}
        </option>
      </select>
    </div>
  </div>
</template>

<script>
import HttpService from "@/services/HttpService.vue";

export default {
  data: function () {
    return {
      cryptoData: {},
      key: "",
      cryptos: [
        { id: 1, name: "Bitcoin", ticker: "BTC" },
        { id: 2, name: "Litecoin", ticker: "LTC" },
        { id: 3, name: "Ethereum", ticker: "ETC" },
        { id: 4, name: "Bitcoin Cash", ticker: "BCH" },
        { id: 5, name: "Dogecoin", ticker: "DOGE" },
        { id: 6, name: "Ethereum Classic", ticker: "ETC" },
      ],
      selectedCrypto: {},
      getCryptoData: { crypto: {}, historicalDate: "01-01-2017", currentDate: "10-06-2021" },
      showData: false,
    };
  },
  watch: {
    selectedCrypto: function (val) {
      this.getCryptoData.crypto = val;
    },
  },
  methods: {
    coinGeckoGetPrice: function (getCryptoData) {
      HttpService.get(
        `https://api.coingecko.com/api/v3/coins/${getCryptoData.crypto.name.toLowerCase()}/history`,
        {
          params: { date: getCryptoData.historicalDate, localization: false },
        },
        (status, data) => {
          this.cryptoData.history = data;
          console.log(this.cryptoData.history);
          this.showData = true;
          console.log(status, data);
        }
      );
    },
  },
};
</script>
