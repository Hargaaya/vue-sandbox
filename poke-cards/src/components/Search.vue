<template>
  <div class="search-container">
    <input v-if="ListLoaded" type="text" placeholder="Ex. Pikachu, Squirtle" @input="filterList" v-on:click="show" />
    <ul class="search-list">
      <li v-for="item in filteredList" :key="item.id" @click="$emit('onSearch', $event.target.innerText)">
        <p v-on:click="clear">{{ item.name }}</p>
      </li>
    </ul>
  </div>
</template>
<script>
import Axios from "axios";

export default {
  name: "SearchMultiWords",
  data() {
    return {
      ListOfPokemons: [],
      ListLoaded: false,
      filteredList: [],
    };
  },
  methods: {
    fetchPokemonList() {
      const listUrl = new URL("https://pokeapi.co/api/v2/pokemon?limit=1126");
      Axios.get(listUrl)
        .then((res) => {
          this.ListOfPokemons = res.data.results;
          this.ListLoaded = true;
        })
        .catch((err) => console.log(err));
    },
    filterList(e) {
      let value = e.target.value;
      let res = this.ListOfPokemons.filter((item) => item.name.slice(0, value.length) == value);
      this.filteredList = res.splice(0, 10);
      if (value.length == 0) this.filteredList = [];
    },
    clear: function (e) {
      e.path[3].firstChild.value = "";
      e.path[2].style.display = "none";
    },
    show: function () {
      this.filteredList = [];
      document.querySelector(".search-list").style.display = "";
    },
  },
  mounted() {
    this.fetchPokemonList();
  },
};
</script>
<style scoped>
.search-container {
  @apply w-96 h-14 mx-auto rounded-lg z-50;
}

input:focus {
  box-shadow: 0 0 0 max(100vh, 100vw) rgba(0, 0, 0, 0.6);
}

input {
  @apply w-full h-full rounded-lg p-3 outline-none bg-white text-gray-700
   shadow-md mx-auto placeholder-gray-500
   font-bold text-center;
}

.search-list {
  @apply w-full flex flex-col rounded-lg overflow-hidden mt-2 shadow-md;
}

.search-list li {
  @apply w-full text-sm overflow-hidden z-50 bg-white
  capitalize cursor-pointer hover:bg-gray-200;
}

.search-list p {
  @apply p-3 font-bold;
}
</style>
