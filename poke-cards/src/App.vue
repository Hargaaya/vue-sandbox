<template>
  <SingleCard v-if="this.loaded" class="card" v-bind:PokemonInformation="pokemonInformation" />
</template>

<script>
import SingleCard from "./components/SingleCard.vue";
import axios from "axios";

let pokeUrl = new URL("https://pokeapi.co/api/v2");
pokeUrl.pathname += "/pokemon";
pokeUrl.pathname += "/25";

export default {
  name: "App",
  components: {
    SingleCard,
  },
  data() {
    return {
      pokemonInformation: [],
      loaded: false,
    };
  },
  methods: {
    fetchData() {
      axios
        .get(pokeUrl)
        .then((res) => {
          this.pokemonInformation = res.data;
          this.loaded = true;
        })
        .catch((err) => console.log("http error message: " + err));
    },
  },
  created() {
    this.fetchData();
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 60px;
}

.card {
  @apply mx-auto my-20;
}
</style>
