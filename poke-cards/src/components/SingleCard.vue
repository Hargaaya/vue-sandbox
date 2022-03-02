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
          <img
            :src="PokemonInformation.sprites.other.home.front_default"
            :alt="'An image of ' + PokemonInformation.name"
            data-atropos-offset="5"
          />
          <span>
            <p>NO: {{ PokemonInformation.id }}</p>
            <p>HT: {{ PokemonInformation.height }}</p>
            <p>WT: {{ PokemonInformation.weight }}</p>
          </span>
        </div>

        <div v-if="abilityLoaded" class="abilities-container">
          <h2>Abilities</h2>
          <p>{{ PokemonInformation.abilites }}</p>
          <ul>
            <li v-for="item in this.abilities" v-bind:key="item.id">
              <h3>{{ item.name }}</h3>
              <p>{{ item.text }}</p>
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
import Colors from "fast-average-color";

export default {
  name: "SingleCard",
  props: {
    PokemonInformation: Object,
  },
  components: { Atropos },
  data() {
    return {
      abilities: [],
      abilityLoaded: false,
    };
  },
  watch: {
    PokemonInformation: function () {
      console.log("prop has changed!");
      this.updateAbility();
      this.updateColors();
    },
  },
  mounted() {
    console.log("mounted");
    this.updateAbility();
    this.updateColors();
  },
  methods: {
    updateColors() {
      new Colors()
        .getColorAsync(this.PokemonInformation.sprites.other.home.front_default)
        .then((color) => {
          document.querySelector(".card-container").style.backgroundColor = color.hex;
        })
        .catch((err) => console.log(err));
    },

    updateAbility() {
      this.abilities = [];

      this.PokemonInformation.abilities.forEach((item) => {
        axios.get(item.ability.url).then((res) => {
          let data = res.data.effect_entries.filter((obj) => obj.language.name == "en");
          let ability = {};

          ability.name = item.ability.name;
          ability.text = data[0].effect.split(".")[0];

          this.abilities.push(ability);
          this.abilityLoaded = true;
        });
      });
    },
  },
};
</script>

<style scoped>
.atropos-container {
  height: 600px;
  width: 400px;
}

.card-container {
  @apply border-8 border-yellow-400 flex flex-col rounded-2xl
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
  background-image: url("https://media2.giphy.com/media/TM8Fu4TrgkNSmXh5cw/giphy.gif?cid=ecf05e47ulmw29k70cms7qdcbkgplaao3nqfqlfkaeufuag3&rid=giphy.gif&ct=g");
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

.abilities-container {
  @apply px-5;
}

.abilities-container h2 {
  @apply text-left font-bold text-lg py-2;
}

.abilities-container li {
  @apply flex flex-row flex-wrap justify-between py-2;
}

.abilities-container h3 {
  @apply font-bold text-center w-full;
}

.abilities-container p {
  @apply text-xs;
}
</style>

/** - - Future backgrounds - -
https://media2.giphy.com/media/dB66K4Kywc8gPVh6A7/giphy.gif?cid=ecf05e47hbgh7xcagu8hvlaeehpj803bqudtiqhq2wiuwn7h&rid=giphy.gif&ct=g BLUE
https://media2.giphy.com/media/xT0xeuC4g0tJu17YSA/giphy.gif?cid=ecf05e47sgv6ibmwve4jlllysublpri5n8qtcurnll8ivw6f&rid=giphy.gif&ct=g YELLOW
https://media4.giphy.com/media/YqhIK6Gbor6CLeloBq/giphy.gif?cid=ecf05e47hkdtil2q088l3g8lvp5zk43htantb6t76s7w7m4u&rid=giphy.gif&ct=g PURPLE
https://media4.giphy.com/media/ZZSY833SbfSorRzpGY/giphy.gif?cid=ecf05e47ox6egga33iafwzie2j4zwl0hckptnzj94wf1gm9g&rid=giphy.gif&ct=g RED
https://media1.giphy.com/media/fYBLMOMuyghpOpCpdY/giphy.gif?cid=ecf05e471n24a6d6v37wrtzqbcqw1uvtqcnnmciqzs9uijso&rid=giphy.gif&ct=g COLOR
https://media3.giphy.com/media/cw5i0cXBLVyff8Oax7/giphy.gif?cid=ecf05e47mvyjaaz6k4g67rwsm3m077575mpmz6iazkfvu58s&rid=giphy.gif&ct=g GREEN
**/
