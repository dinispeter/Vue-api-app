<template>
  <div id="app">
    <h1> {{name}} api </h1>
    <br />

    <section class="info">
      <p>Info about this api: {{newInfo}}</p>
      <p>Udpated at: {{time}}</p>
    </section>

    <section v-if="errored">
      <p>
          <b>
          We're sorry, we're not able to retrieve this information at the moment,
          please try back later
          </b>
      </p>
    </section>

    <section v-else class="money">
      <div v-if="loading">Loading...</div>

      <div v-else v-for="currency in info" class="currency">
        {{ currency.description }}:
        <span class="lighten">
          <span v-html="currency.symbol"></span>
          {{ currency.rate_float | currencydecimal }}
        </span>
      </div>
    </section>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      info: null,
      name: null,
      loading: true,
      errored: false,
      newInfo: null,
      time: null
    };
  },

  // using vue lifecycle hook mounted
  filters: {
    currencydecimal(value) {
      return value.toFixed(2);
    }
  },
  mounted() {
    axios
      .get("https://api.coindesk.com/v1/bpi/currentprice.json")
      .then(response => {
        (
          this.info = response.data.bpi), (this.name = response.data.chartName), (this.newInfo = response.data.disclaimer), (this.time = response.data.time.updated);
      })
      .catch(error => {
        console.log(error);
        this.errored = true;
      })
      .finally(() => (this.loading = false));
  }
};
</script>

<style lang="scss">
  $base-color: #006699;
  $font-base: "Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-seri";
  $secondary-color: #333;


  * {
    font-family: $font-base;
    padding: 0;
    margin: 0;
  }

  h1 {
    color: $base-color;
    text-align: center;
    text-decoration: underline;
  }

  .info {
    font-family: $font-base;
    color: $secondary-color;
    margin-left: 20px;
    margin-right: 20px;

  }

  .money {
    font-family: $font-base;
    color: $secondary-color;
    margin-left: 20px;
    margin-right: 20px;
  }
</style>
