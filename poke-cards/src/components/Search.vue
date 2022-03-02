<template>
  <div class="search-container">
    <input v-if="ListLoaded" type="text" placeholder="Search" @input="filterList" />
    <ul class="search-list">
      <li v-for="item in filteredList" :key="item.id" @click="$emit('onSearch', $event.target.innerText)">
        {{ item.name }}
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
  },
  mounted() {
    this.fetchPokemonList();
  },
};
</script>
<style scoped>
.search-container {
  @apply w-80 h-14 mx-auto;
}

input {
  @apply w-72 rounded-xl p-2 m-2 outline-none bg-transparent text-white
   shadow-inner mx-auto border-8 border-white placeholder-white
   font-bold text-center;
}

.search-list {
  @apply flex flex-col rounded-2xl w-72 mx-auto overflow-hidden;
}

.search-list li {
  @apply text-sm border-b-2 border-gray-300 p-2 overflow-hidden z-50 bg-gray-50 w-72
  capitalize;
}
</style>
