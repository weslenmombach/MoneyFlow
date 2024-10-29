<script setup>
import { ref, onMounted } from "vue";

const currencies = ref([]); // Inicializa como um array vazio para busca na API
const show = ref(false);
const apiUrl =
  "https://v6.exchangerate-api.com/v6/20c06da9f2139de4f408f101/latest/USD";

// Função para determinar a manipulação do botão
function toggleCurrencies() {
  show.value = !show.value;
}

// Função para buscar as cotações
async function fetchCurrencies() {
  try {
    const response = await fetch(apiUrl); // Usa a constante apiUrl
    if (!response.ok) {
      throw new Error("Network response was not ok");
    }
    const data = await response.json();
    currencies.value = Object.entries(data.conversion_rates); // Converte o objeto de taxas em um array de pares [moeda, taxa]
  } catch (error) {
    console.error("Erro ao buscar cotações:", error);
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
        <button
          class="navbar-toggler"
          type="button"
          data-bs-toggle="collapse"
          data-bs-target="#navbarScroll"
          aria-controls="navbarScroll"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarScroll">
          <ul
            class="navbar-nav me-auto my-2 my-lg-0 navbar-nav-scroll"
            style="--bs-scroll-height: 100px"
          >
            <li class="nav-item">
              <a class="nav-link active" aria-current="page" href="#">Home</a>
            </li>

            <li class="nav-item dropdown">
              <a
                class="nav-link dropdown-toggle"
                href="#"
                role="button"
                data-bs-toggle="dropdown"
                aria-expanded="false"
              >
                Menu
              </a>
              <ul class="dropdown-menu">
                <li><a class="dropdown-item" href="#">Select</a></li>
                <li><a class="dropdown-item" href="#">Calculate</a></li>
              </ul>
            </li>
          </ul>
          <form class="d-flex" role="search">
            <input
              class="form-control me-2"
              type="search"
              placeholder="Search Currencie"
              aria-label="Search"
            />
            <button class="btn btn-outline-success" type="submit">
              Search
            </button>
          </form>
        </div>
      </div>
    </nav>

    <h1 class="title">MoneyFlow</h1>
  </header>

<main class="moneyPage">
  <div class="totalContent">
    <div class="topSections">
      <section class="select_currencies">
        <h2 class="title_main">Select currency</h2>
        <hr />
        <div class="currencies">
          <button v-if="!show" @click="toggleCurrencies">Show Currencies</button>
          <button v-if="show" @click="toggleCurrencies">Hide Currencies</button>
          <ul v-if="show">
            <li v-for="[currency, rate] in currencies" :key="currency">
              {{ currency }}: {{ rate }}
            </li>
          </ul>
        </div>
      </section>
      <section class="calculator">
        <div class="calculator_page">
          <h2 class="title_main">Calculate currency</h2>
          <hr />
        </div>
      </section>
    </div>

    <section class="menu">
      <div class="card text-center">
        <div class="card-header">
          <ul class="nav nav-tabs card-header-tabs">
            <li class="nav-item">
              <a class="nav-link active" aria-current="true" href="#">How to use</a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="#">Why to use</a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="#">Questions</a>
            </li>
          </ul>
        </div>
        <div class="card-body">
          <h5 class="card-title">Special title treatment</h5>
          <p class="card-text">With supporting text below as a natural lead-in to additional content.</p>
          <a href="#" class="btn btn-primary">Go somewhere</a>
        </div>
      </div>
    </section>
  </div>
</main>

  <!--FOOTER-->
  <footer class="text-center text-lg-start">
    <div>
      <ul class="ul_footer">
        <li><a href="#">Home</a></li>
        <li><a href="#">FAQ</a></li>
        <li><a href="#">Contact</a></li>
      </ul>
    </div>
    <p class="copy_footer">
      Desenvolvido por&nbsp;<a
        href="https://github.com/weslenmombach"
        target="_blank"
        >Weslen Mombach</a
      >&nbsp;<span>&copy;</span>
    </p>
  </footer>
</template>

<style>
* {
  margin: 0px;
  padding: 0px;
  font-family: "Roboto", sans-serif;
}
.title {
  text-align: center;
  font-weight: 800;
  font-family: "Roboto Slab", serif;
  padding: 40px 0px 80px 80px;
  display: flex;
  justify-content: left;
  font-size: 4em;
  background-color: #1e90ff;
  margin: 0px;
  color: white;
}
.btn-outline-success{
  color: #0a3d62;
  border-color: #0a3d62;
}

.btn-outline-success:hover{
  background-color: #1e90ff;
  border-color: white;
}

/*CONTENT MAIN*/
.totalContent {
  display: flex;
  justify-content: space-between;
  flex-direction: column;
  align-items: center;
  background: linear-gradient(to top, #0a3d62, #1e90ff);
  min-height: 100vh;
}

.topSections {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 60%;

  margin-bottom: 20px;
}

main h2 {
  text-align: center;
}

.menu {
  display: flex;
  justify-content: center;
  width: 80%;
  max-width: 600px;
}

.select_currencies, .calculator {
  border: 2px solid white;
  border-radius: 30px;
  box-shadow: 5px 5px 20px rgba(0, 0, 0, 0.5);
  width: 28vw;
  margin: 20px 0px; 
  height: 70vh;
}

.title_main {
  padding-top: 20px;
  text-align: center;
  font-family: "Roboto Slab", serif;
  color: white;
}

/* FOOTER */
footer {
  display: flex;
  flex-direction: column;
  align-items: center;
  bottom: 0;
  left: 0;
  width: 100%;
  background-color: #0a3d62;
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

.ul_footer >li > a{
  text-decoration: none;
  color: white;
}

.ul_footer >li > a:hover{
  text-decoration: underline;
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

.copy_footer > a{
  text-decoration: none;
  color: white;
}

.copy_footer > a:hover {
  text-decoration: underline;
}

/* RESPONSIVIDADE PRA DIFERENTES TELAS */
@media (max-width: 1280px) {
  .moneyPage {
    flex-direction: column;
    align-items: center;
  }

  .select_currencies,
  .calculator {
    width: 80%;
    height: 100vh;
  }
}
</style>
