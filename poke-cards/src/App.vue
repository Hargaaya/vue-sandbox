<template>
  <div class="container">
    <Search @onSearch="onSearch" />
    <SingleCard v-if="this.loaded" class="card" v-bind:PokemonInformation="pokemonInformation" />
  </div>
</template>

<script>
import SingleCard from "./components/SingleCard.vue";
import Search from "./components/Search.vue";
import axios from "axios";

const pokeUrl = new URL("https://pokeapi.co/api/v2");
pokeUrl.pathname += "/pokemon/";

export default {
  name: "App",
  components: {
    SingleCard,
    Search,
  },
  data() {
    return {
      pokemonInformation: [],
      loaded: false,
    };
  },
  methods: {
    onSearch(pokemonName) {
      this.fetchData(pokemonName.toLowerCase());
    },

    fetchData(pokemon) {
      axios
        .get(pokeUrl + pokemon)
        .then((res) => {
          this.pokemonInformation = res.data;
          this.loaded = true;
        })
        .catch((err) => this.failedFetch(err));
    },

    failedFetch(msg) {
      msg;
      //alert("That pokemon does not exist :(");
      //console.log("Message: " + msg);
    },
  },
  created() {
    this.fetchData("pikachu");
  },
};
</script>

<style>
#app {
  font-family: "Open Sans", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 60px;
}

.container {
  @apply flex flex-col justify-center items-center;
}

.card {
  @apply mx-auto my-20;
}
</style>
