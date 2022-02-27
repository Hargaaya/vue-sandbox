<template>
  <SingleCard
    v-bind:CardInfo="{
      name: 'Blaustoise',
      hp: 50,
      type: 'Water',
      image: 'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/home/shiny/1.png',
      number: 3,
      height: 30,
      weight: 50,
      attacks: 'Literally a wall of text that will be fetched by the api',
    }"
  />
  <AtroposTesting />
</template>

<script>
import SingleCard from "./components/SingleCard.vue";
import AtroposTesting from "./components/AtroposTesting.vue";

export default {
  name: "App",
  components: {
    SingleCard,
    AtroposTesting,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
