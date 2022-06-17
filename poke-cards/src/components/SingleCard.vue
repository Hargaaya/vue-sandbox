<template>
  <atropos class="atropos-container">
    <div class="card-container" data-atropos-offset="0">
      <header data-atropos-offset="2">
        <h3>{{ PokemonInformation.name }}</h3>
        <span>
          <p><b>HP</b> {{ PokemonInformation.stats[0].base_stat }}</p>
          <i class="fa globe-bg" :class="typeIcon"></i>
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
            <p>NO. {{ PokemonInformation.id }}</p>
            <p>HT: {{ PokemonInformation.height }}</p>
            <p>WT: {{ PokemonInformation.weight }}</p>
          </span>
        </div>

        <div v-if="movesLoaded" class="moves-container" data-atropos-offset="2">
          <ul>
            <li v-for="item in this.moves" v-bind:key="item.id">
              <span>
                <i class="fa globe-bg" :class="typeIcon"></i>
                <h3>{{ item.name }}</h3>
                <b> {{ item.power }}</b>
              </span>
              <p>{{ item.effect }}</p>
            </li>
          </ul>
        </div>
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
      moves: [],
      movesLoaded: false,
      typeIcon: "",
    };
  },
  watch: {
    PokemonInformation: function () {
      this.updateMoves();
      this.updateColors();
      this.updateTypeIcon();
    },
  },
  mounted() {
    this.updateMoves();
    this.updateColors();
    this.updateTypeIcon();
  },
  methods: {
    updateColors() {
      new Colors()
        .getColorAsync(this.PokemonInformation.sprites.other.home.front_default)
        .then((color) => {
          document.querySelector(".card-container").style.backgroundColor = color.hex;
          document.querySelector(".image-container").style.backgroundColor = color.hex;
          // document.querySelectorAll("fa").forEach((item) => {
          //   item.style.color = color.hex;
          // });
        })
        .catch((err) => console.log(err));
    },

    updateMoves() {
      this.moves = [];

      let numberOfMoves = this.PokemonInformation.moves.length;

      for (let i = 0; i < 2; i++) {
        let item = this.PokemonInformation.moves;
        let j = Math.floor(numberOfMoves / 4 + i);

        axios.get(item[j].move.url).then((res) => {
          let data = res.data;
          let move = {};

          move.name = item[j].move.name.replace("-", " ");
          move.effect = data.flavor_text_entries.filter((obj) => obj.language.name == "en")[4].flavor_text;
          move.power = data.power;

          this.moves.push(move);
          this.movesLoaded = true;
        });
      }
    },

    updateTypeIcon() {
      let type = this.PokemonInformation.types[0].type.name;
      let iconList = {
        normal: "fa-asterisk bg-indigo-300",
        fire: "fa-fire bg-yellow-500",
        water: "fa-water bg-blue-400",
        grass: "fa-leaf bg-green-500",
        electric: "fa-bolt bg-yellow-400",
        ice: "fa-snowflake bg-blue-300",
        fighting: "fa-hand-fist bg-red-600",
        poison: "fa-biohazard bg-purple-600",
        ground: "fa-earth-africa bg-yellow-600",
        flying: "fa-dove bg-pink-400",
        psychic: "fa-brain bg-pink-500",
        bug: "fa-bug bg-green-400",
        rock: "fa-gem bg-yellow-700",
        ghost: "fa-ghost bg-purple-400",
        dark: "fa-moon bg-yellow-900",
        dragon: "fa-dragon bg-purple-500",
        steel: "fa-shield bg-gray-400",
        fairy: "fa-wand-magic-sparkles bg-pink-300",
      };
      this.typeIcon = iconList[type];
    },
  },
};
</script>

<style scoped>
.atropos-container {
  @apply mx-auto mt-20;
  height: 600px;
  width: 400px;
}

i {
  font-size: 14px;
  text-align: center;
}

.backgrounds {
  @apply bg-red-500 bg-gray-400 bg-green-500 bg-yellow-500 bg-pink-300 
  bg-purple-500 bg-yellow-900 bg-yellow-700 bg-green-400 bg-pink-500
  bg-pink-400 bg-yellow-600 bg-purple-600 bg-blue-400 bg-blue-300 bg-red-600 
  bg-indigo-300;
}

h3 {
  @apply capitalize font-semibold m-2 overflow-hidden;
}

.card-container {
  @apply border-8 border-yellow-400 flex flex-col rounded-2xl
  select-none w-full h-full overflow-hidden;
  background-image: url(../assets/clean-gray-paper.png), url(../assets/sparkle.webp);
  background-blend-mode: soft-light;
  filter: brightness(0.9);
}

.card-container:hover,
.card-container:active {
  background-image: url(../assets/clean-gray-paper.png), url(../assets/sparkles.gif);
  background-blend-mode: soft-light;
  filter: saturation(1.1) brightness(1);
}

header {
  @apply flex flex-row justify-between items-center h-10 mt-2 mx-5 font-bold;
}

header h3 {
  @apply font-bold;
}

header span {
  @apply flex w-24 justify-around items-center;
}

header p > * {
  @apply font-bold text-xs;
}

.image-container {
  @apply mx-auto mt-2 mb-8 flex flex-col justify-center items-center shadow-lg;
  /* background-color: #ffffff; */
  background-image: url(../assets/trippy-circle.webp), url(../assets/stars.webp);
  background-blend-mode: overlay;
  background-position: center;
  height: 200px;
  width: 90%;
}

.image-container img {
  @apply h-40 w-40 z-10;
}

.image-container span {
  @apply flex justify-evenly transform skew-x-12 w-80 mt-6 italic text-xs
  shadow-lg border-2 border-gray-400 bg-gradient-to-t from-gray-400 to-gray-100;
}

.moves-container {
  @apply px-5;
}

.moves-container span {
  @apply flex flex-row justify-between mb-2;
}
.moves-container p {
  @apply text-left text-sm pb-1.5 mb-3;
}

.globe-bg {
  @apply w-7 h-7 pt-1.5 rounded-full;
  filter: contrast(4);
  box-shadow: inset -7px -4px 12px rgba(0, 0, 0, 0.3);
  background-image: linear-gradient(-45deg, rgba(255, 255, 220, 0.3) 0%, transparent 100%);
}

@media screen and (max-width: 420px) {
  .atropos-container {
    zoom: 0.9;
    -moz-transform: scale(0.9);
  }
}
@media screen and (max-width: 380px) {
  .atropos-container {
    zoom: 0.8;
    -moz-transform: scale(0.8);
  }
}
</style>

/** - - Future backgrounds? - -
https://media2.giphy.com/media/dB66K4Kywc8gPVh6A7/giphy.gif?cid=ecf05e47hbgh7xcagu8hvlaeehpj803bqudtiqhq2wiuwn7h&rid=giphy.gif&ct=g BLUE
https://media2.giphy.com/media/xT0xeuC4g0tJu17YSA/giphy.gif?cid=ecf05e47sgv6ibmwve4jlllysublpri5n8qtcurnll8ivw6f&rid=giphy.gif&ct=g YELLOW
https://media4.giphy.com/media/YqhIK6Gbor6CLeloBq/giphy.gif?cid=ecf05e47hkdtil2q088l3g8lvp5zk43htantb6t76s7w7m4u&rid=giphy.gif&ct=g PURPLE
https://media4.giphy.com/media/ZZSY833SbfSorRzpGY/giphy.gif?cid=ecf05e47ox6egga33iafwzie2j4zwl0hckptnzj94wf1gm9g&rid=giphy.gif&ct=g RED
https://media1.giphy.com/media/fYBLMOMuyghpOpCpdY/giphy.gif?cid=ecf05e471n24a6d6v37wrtzqbcqw1uvtqcnnmciqzs9uijso&rid=giphy.gif&ct=g COLOR
https://media3.giphy.com/media/cw5i0cXBLVyff8Oax7/giphy.gif?cid=ecf05e47mvyjaaz6k4g67rwsm3m077575mpmz6iazkfvu58s&rid=giphy.gif&ct=g GREEN
**/
