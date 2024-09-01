<script setup>
import { RouterLink, RouterView } from 'vue-router'
import Title from './components/Title.vue'
</script>

<template>
  <header>
    <div class="wrapper">
      <Title msg="Конвертер валют" />

      <nav>
        <RouterLink to="/">Главная</RouterLink>
        <RouterLink to="/convert">Конвертация</RouterLink>
      </nav>
      <div class="currency">
        <label class="green label">Валюта</label>
        <select v-model="baseCurrency"  @change="changeBaseCurrency">
          <option v-for="currency in currencies" :key="currency" :value="currency">{{currency.toUpperCase()}}</option>
        </select>
      </div>
    </div>
  </header>

  <RouterView :base-currency="baseCurrency" :currencies="currencies"/>
</template>

<script>
export default {
  data() {
    return {
      baseCurrency: 'rub',
      currencies: ['rub', 'usd', 'eur']
    };
  },
  methods: {
    changeBaseCurrency() {
      this.$emit('base-currency-changed', this.baseCurrency);
    }
  }
};
</script>

<style scoped>
header {
  line-height: 1.5;
  max-height: 100vh;
  margin-bottom: 3rem;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

nav {
  width: 100%;
  font-size: 20px;
  text-align: center;
  margin-top: 2rem;
}

nav a.router-link-exact-active {
  color: var(--color-green);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  color: var(--color-text);
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

.currency {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-top: 2rem;
  gap: 1rem;
}

.label {
  font-size : 1.3rem;
  color: var(--color-text);
}

select {
  font-size: 1rem;
  padding: 0.4rem;
  border-radius: 0.4rem;
}


@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
    margin-bottom: 0;
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  nav {
    text-align: left;
    font-size: 1.6rem;
  }

  .currency {
    margin-left: 1rem;
  }
}
</style>
