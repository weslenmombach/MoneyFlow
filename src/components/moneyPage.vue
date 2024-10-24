<script setup>
import { ref, onMounted } from 'vue';

const currencies = ref([]); // Inicializa como um array vazio para busca na API
const show = ref(false);
const apiUrl = 'https://v6.exchangerate-api.com/v6/20c06da9f2139de4f408f101/latest/USD'; 

// Função para determinar a manipulação do botão
function toggleCurrencies() {
  show.value = !show.value; 
}

// Função para buscar as cotações
async function fetchCurrencies() {
  try {
    const response = await fetch(apiUrl); // Usa a constante apiUrl
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    const data = await response.json();
    currencies.value = Object.entries(data.conversion_rates); // Converte o objeto de taxas em um array de pares [moeda, taxa]
  } catch (error) {
    console.error('Erro ao buscar cotações:', error);
  }
}

// Executa a função fetchCurrencies quando o componente é montado
onMounted(fetchCurrencies);
</script>

<template>
  <section class="moneyPage">
      <h2>Here I'll make a new page to show different countries and money options</h2>
      <div class="currencies">
        <button v-if="!show" @click="toggleCurrencies">Show Currencies</button>
        <button v-if="show" @click="toggleCurrencies">Hide Currencies</button>
        <!-- Utilização de toggle para manipular o button -->
        <ul v-if="show">
          <li v-for="[currency, rate] in currencies" :key="currency">{{ currency }}: {{ rate }}</li>
        </ul>
      </div>
  </section>
</template>

<style>
</style>
