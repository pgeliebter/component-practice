<template>
  <div class="home">
    <button v-on:click="coinGeckoGetPrice(getCryptoData)">Get Data</button>

    <div>
      <div>
        <label for="oldDate">Historical Date</label>

        <input
          v-model="getCryptoData.historicalDate"
          type="text"
          id="oldDate"
        />
      </div>
      <select v-model="selectedCrypto">
        <option
          v-bind:value="crypto"
          v-for="crypto in cryptos"
          v-bind:key="crypto.id"
        >
          {{ crypto.name }}
        </option>
      </select>
      <!-- <div>{{ getResponse }}</div> -->
    </div>
    <div class="container">
      <div v-if="showData === true">
        <LineChart></LineChart>
      </div>
    </div>
  </div>
</template>

<script>
import HttpService from '@/services/HttpService.vue'
import LineChart from '@/components/LineChart.vue'

export default {
  components: { LineChart },
  data: function () {
    return {
      getResponse: {},
      key: '',
      cryptos: [
        { id: 1, name: 'Bitcoin', ticker: 'BTC' },
        { id: 2, name: 'Litecoin', ticker: 'LTC' },
        { id: 3, name: 'Ethereum', ticker: 'ETC' },
        { id: 4, name: 'Bitcoin Cash', ticker: 'BCH' },
        { id: 5, name: 'Dogecoin', ticker: 'DOGE' },
        { id: 6, name: 'Ethereum Classic', ticker: 'ETC' },
      ],
      selectedCrypto: {},
      getCryptoData: {
        crypto: {},
        historicalDate: '01-01-2017',
        currentDate: '10-06-2021',
      },
      showData: true,
    }
  },
  watch: {
    selectedCrypto: function (val) {
      this.getCryptoData.crypto = val
    },
  },
  methods: {
    coinGeckoGetPrice: function (getCryptoData) {
      HttpService.get(
        `https://api.coingecko.com/api/v3/coins/${getCryptoData.crypto.name.toLowerCase()}/market_chart/range`,
        {
          params: {
            vs_currency: 'usd',
            from: new Date(getCryptoData.historicalDate).getTime() / 1000,
            to: new Date(getCryptoData.currentDate).getTime() / 1000,
          },
        },
        (status, data) => {
          this.getResponse = data

          console.log(status, data)
        }
      )
    },
  },
}
</script>
