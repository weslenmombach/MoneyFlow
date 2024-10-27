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
  <!--HEADER-->
  <header>
    <nav class="navbar navbar-expand-lg bg-body-tertiary">
      <div class="container-fluid">
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarScroll" aria-controls="navbarScroll" aria-expanded="false" aria-label="Toggle navigation">
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarScroll">
          <ul class="navbar-nav me-auto my-2 my-lg-0 navbar-nav-scroll" style="--bs-scroll-height: 100px;">
            <li class="nav-item">
              <a class="nav-link active" aria-current="page" href="#">Home</a>
            </li>

            <li class="nav-item dropdown">
              <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                Options
              </a>
              <ul class="dropdown-menu">
                <li><a class="dropdown-item" href="#">Action</a></li>
                <li><a class="dropdown-item" href="#">Another action</a></li>
                <li><hr class="dropdown-divider"></li>
                <li><a class="dropdown-item" href="#">Something else here</a></li>
              </ul>
            </li>

          </ul>
          <form class="d-flex" role="search">
            <input class="form-control me-2" type="search" placeholder="Search Currencie" aria-label="Search">
            <button class="btn btn-outline-success" type="submit">Search</button>
          </form>
        </div>
      </div>
    </nav>
    
    <h1 class="title">MoneyFlow</h1>
  </header>


  <main class="moneyPage">
      <section class="select_currencies">
        <h2 class="title_main">Select the currencie</h2>
        <hr>
        <div class="currencies">
          <button v-if="!show" @click="toggleCurrencies">Show Currencies</button>
          <button v-if="show" @click="toggleCurrencies">Hide Currencies</button>
          <!-- Utilização de toggle para manipular o button -->
          <ul v-if="show">
            <li v-for="[currency, rate] in currencies" :key="currency">{{ currency }}: {{ rate }}</li>
          </ul>
        </div>
      </section>

      <section class="calculator">
        <div class="calculator_page">
          <h2 class="title_main">
            Calculate your currencie
          </h2>
          <hr>
        </div>
      </section>
  </main>

<!--FOOTER-->
  <footer class="text-center text-lg-start">
    <div>
      <ul class="ul_footer">
        <li>Home</li>
        <li>FAQ</li>
        <li>Contact</li>
      </ul>
    </div>
    <p class="copy_footer">Desenvolvido por&nbsp;<a href="https://github.com/weslenmombach" target="_blank">Weslen Mombach</a>&nbsp;<span>&copy;</span></p>

  </footer>
</template>

<style>
*{
  margin: 0px;
  padding: 0px;
  font-family: 'Roboto', sans-serif;
}
.title{
  text-align: center;
  font-weight: bolder;
  padding: 80px;
}

/*CONTENT MAIN*/
.moneyPage{
  display: flex;
  justify-content: space-around;
  height: 100vh;
  background: linear-gradient(to top, #0A3D62,#1E90FF);
}

main h2{
  text-align: center;
}

.select_currencies{
  border: 2px solid white;
  border-radius: 30px;
  box-shadow: 5px 5px 20px rgba(0, 0, 0, 0.5);
  width: 28vw;
  margin: 40px 0px;
}

.calculator{
  border: 2px solid white;
  border-radius: 30px;
  box-shadow: 5px 5px 20px rgba(0, 0, 0, 0.5);
  width: 28vw;
  margin: 40px 0px;
}

.title_main{
  padding-top: 20px;
}

/* FOOTER */
footer {
  display: flex;
  flex-direction: column;
  align-items: center;
  bottom: 0;
  left: 0;
  width: 100%;
  background-color: #0A3D62;
  color: white;
  padding-top: 100px;
}

.ul_footer {
  list-style-type: none;
  display: flex;
  justify-content: space-between;
  max-width: 300px;
  width: 100%; 
  margin: 0 auto; 
  padding: 0;
}

.ul_footer > li {
  margin: 10px;
}

.copy_footer {
  text-decoration: none;
  color: white;
  display: flex;
  justify-content: center;
  margin-top: 10px;
}

.copy_footer >a:hover {
  text-decoration: underline;
}

/* RESPONSIVIDADE PRA DIFERENTES TELAS */
@media (max-width: 1280px) {
  .moneyPage {
    flex-direction: column;
    align-items: center;
  }

  .select_currencies, .calculator {
    width: 80%;
    height: 100vh;
  }
}
</style>
