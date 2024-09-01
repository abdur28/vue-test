<template>
  <div class="home">
    <h1 class="green">Курсы валют</h1>
    <ul>
      <li v-for="(rate, currency) in rates" :key="currency">
        <span class="green">1</span> {{ currency.toUpperCase() }} = <span class="green">{{ (rate).toFixed(2) }}</span> {{ baseCurrency.toUpperCase() }}
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  props: ['baseCurrency', 'currencies'],
  data() {
    return {
      rates: {}
    };
  },
  watch: {
    baseCurrency: 'fetchRates'
  },
  mounted() {
    this.fetchRates();
  },
  methods: {
    async fetchRates() {
      this.rates = {};
      const response = await axios.get('https://status.neuralgeneration.com/api/currency');
      this.currencies.forEach((currency) => {
        if ( currency !== this.baseCurrency) {
          const exchange = currency + "-" + this.baseCurrency 
          this.rates[currency] = response.data[exchange];
        }
      })
    }
  }
};
</script>

<style scoped>
.home {
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 1rem;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  font-size: 1.3rem; 
}

@media (min-width: 1024px) {
  .home {
    text-align: left;
    justify-content: flex-start;
    align-items: flex-start;
  }
}
</style>
