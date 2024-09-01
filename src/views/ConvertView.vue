<template>
  <div class="convert">
    <h1 class="green">Конвертация валюты</h1>
    <div class="currencyInput">
      <select v-model="currencyFrom" class="currencySelect" @change="setCurrentRate('from')">
        <option v-for="currency in currencies" :key="currency" :value="currency">
          {{ currency.toUpperCase() }}
        </option>
      </select>
      <input type="number" v-model.number="amountFrom" @input="updateRate('from')" />
    </div>
    <div class="currencyInput">
      <select v-model="currencyTo" class="currencySelect" @change="setCurrentRate('to')">
        <option v-for="currency in currencies" :key="currency" :value="currency">
          {{ currency.toUpperCase() }}
        </option>
      </select>
      <input type="number" v-model.number="amountTo" @input="updateRate('to')" />
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  props: ['baseCurrency', 'currencies'],
  data() {
    return {
      currencyFrom: this.baseCurrency,
      currencyTo: this.currencies.find((currency) => currency !== this.baseCurrency),
      currentRate: 0,
      amountFrom: 1,
      amountTo: 0,
      rates: {}
    };
  },
  mounted() {
    this.fetchRates();
  },
  methods: {
    async fetchRates() {
      const response = await axios.get('https://status.neuralgeneration.com/api/currency');
      this.currencies.forEach((currency) => {
        this.currencies.forEach((currency2) => {
          if(currency !== currency2) {
            const exchange = currency + "-" + currency2
            this.rates[exchange] = response.data[exchange];
          }
        })
        this.rates[currency + "-" + currency] = 1
      })
      this.currentRate = this.rates[this.currencyFrom + '-' + this.currencyTo];
      this.amountTo = (this.amountFrom * this.currentRate).toFixed(2);
    },
    setCurrentRate(direction) {
      this.currentRate = this.rates[this.currencyFrom + '-' + this.currencyTo];
      if (direction === 'from') {
        this.amountTo = (this.amountFrom * this.currentRate).toFixed(2);
      } else {
        this.amountFrom = (this.amountTo / this.currentRate).toFixed(2);
      }
    },
    updateRate(direction) {
      if (direction === 'from') {
        this.amountTo = (this.amountFrom * this.currentRate).toFixed(2);
      } else if (direction === 'to') {
        this.amountFrom = (this.amountTo / this.currentRate).toFixed(2);
      }
    }
  }
};
</script>

<style>
.convert {
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 1rem;
}

.currencyInput {
  display: flex;
  background: transparent;
  border: 1px solid var(--color-text);
  border-radius: 0.5rem;
}

input[type=number] {
  -moz-appearance: textfield;
}

input[type=number]::-webkit-inner-spin-button,
input[type=number]::-webkit-outer-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

input {
  background: transparent;
  border: none;
  padding: 0.5rem;
  font-size: 1.2rem;
  color: var(--color-text);
  text-align: right;
  width: 100%;
}

input:focus {
  outline: none;
}


.currencySelect {
  outline: none;
  border-bottom-left-radius: 0.5rem;
  border-top-left-radius: 0.5rem;
}

@media (min-width: 1024px) {
  .convert {
    text-align: left;
    justify-content: flex-start;
    align-items: flex-start;
  }
}
</style>
