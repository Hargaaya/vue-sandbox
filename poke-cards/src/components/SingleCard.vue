<template>
  <atropos class="atropos-container">
    <div class="card-container" data-atropos-offset="0">
      <header>
        <h3>{{ PokemonInformation.name }}</h3>
        <span>
          <p><b>HP</b> {{ PokemonInformation.stats[0].base_stat }}</p>
          <i class="fa fa-bolt"></i>
        </span>
      </header>

      <div class="card-body">
        <div class="image-container" data-atropos-offset="3">
          <img :src="PokemonInformation.sprites.other.home.front_default" :alt="PokemonInformation.name" data-atropos-offset="3" />
          <span>
            <p>NO: {{ PokemonInformation.id }}</p>
            <p>HT: {{ PokemonInformation.height }}</p>
            <p>WT: {{ PokemonInformation.weight }}</p>
          </span>
        </div>

        <div class="attacks-container">
          <h2>Attacks</h2>
          <p>{{ PokemonInformation.abilites }}</p>
          <ul v-for="ability in PokemonInformation.abilities" v-bind:key="ability.id">
            <li>
              <h3>{{ ability.ability.name }}</h3>
              <a :href="ability.ability.url">Link</a>
            </li>
          </ul>
        </div>
        <span v-for="type in PokemonInformation.types" :key="type.id">
          <h3>{{ type.type.name }}</h3>
        </span>
      </div>
    </div>
  </atropos>
</template>
<script>
import Atropos from "atropos/vue";
import axios from "axios";

export default {
  name: "SingleCard",
  props: {
    PokemonInformation: Object,
  },
  components: { Atropos },
  data() {
    return {
      abilities: [],
    };
  },
  method: {
    getAbilities() {
      this.PokemonInformation.abilites.forEach((ability) => {
        console.log(ability.url);
      });
    },
    rnd: function () {
      return Math.floor(Math.random() * 4);
    },

    fetchAbility(url) {
      axios
        .get(url)
        .then((res) => {
          this.abilities += res.data;
        })
        .catch((err) => console.log("MASSIVE ERROR!!! (REAL) (NOT FAKE)" + err));
    },
  },
  created() {
    console.log(this.PokemonInformation);
  },
};
</script>

<style scoped>
.atropos-container {
  height: 600px;
  width: 400px;
}

.card-container {
  @apply bg-yellow-500 border-8 border-yellow-400 flex flex-col rounded-2xl
  select-none w-full h-full;
  background-image: url("https://www.transparenttextures.com/patterns/clean-gray-paper.png");
}

header {
  @apply flex flex-row justify-between items-center h-10 mt-1 mx-5 font-bold;
}

header span {
  @apply flex w-24 justify-around items-center;
}

header i {
  @apply w-8 h-8 p-1.5 rounded-full border border-black;
}

header p > * {
  @apply font-bold text-xs;
}

.image-container {
  @apply border-4 border-gray-400 mx-auto mt-5 mb-10 flex flex-col justify-center items-center;
  background-image: url("https://media2.giphy.com/media/xT0xeuC4g0tJu17YSA/giphy.gif?cid=ecf05e47sgv6ibmwve4jlllysublpri5n8qtcurnll8ivw6f&rid=giphy.gif&ct=g");
  background-size: cover;
  background-position: center;
  height: 200px;
  width: 300px;
}

.image-container img {
  @apply h-40 w-40;
}

.image-container span {
  @apply flex justify-evenly rounded-full w-80 mt-6 font-bold italic text-sm
  bg-gradient-to-bl from-gray-400 to-white;
}

.attacks-container {
  @apply px-5;
}

.attacks-container h2 {
  @apply text-left font-bold text-lg py-2;
}

.attacks-container li {
  @apply flex flex-row flex-wrap justify-between py-2;
}

.attacks-container h3 {
  @apply font-light;
}
</style>
