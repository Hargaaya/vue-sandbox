<template>
  <atropos class="atropos-container">
    <div class="card-container" data-atropos-offset="0">
      <header data-atropos-offset="2">
        <h3>{{ PokemonInformation.name }}</h3>
        <span>
          <p><b>HP</b> {{ PokemonInformation.stats[0].base_stat }}</p>
          <i class="fa fa-bolt globe-bg"></i>
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
                <i class="fa fa-bolt globe-bg"></i>
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
    };
  },
  watch: {
    PokemonInformation: function () {
      this.updateMoves();
      this.updateColors();
    },
  },
  mounted() {
    this.updateMoves();
    this.updateColors();
  },
  methods: {
    updateColors() {
      new Colors()
        .getColorAsync(this.PokemonInformation.sprites.other.home.front_default)
        .then((color) => {
          document.querySelector(".card-container").style.backgroundColor = color.hex;
          document.querySelectorAll("fa").forEach((item) => {
            item.style.color = color.hex;
          });
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
  },
};
</script>

<style scoped>
.atropos-container {
  height: 600px;
  width: 400px;
}

h3 {
  @apply capitalize font-semibold;
}

.card-container {
  @apply border-8 border-yellow-400 flex flex-col rounded-2xl
  select-none w-full h-full overflow-hidden;
  background-image: url("https://www.transparenttextures.com/patterns/clean-gray-paper.png");
  filter: saturate(1.2);
}

.card-container:hover {
  background-image: url("https://www.transparenttextures.com/patterns/clean-gray-paper.png"), url(../assets/sparkles.gif);
  background-blend-mode: color-dodge;
  filter: contrast(1);
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
  @apply border-4 border-gray-400 mx-auto mt-2 mb-10 flex flex-col justify-center items-center;
  background-image: url("https://media2.giphy.com/media/TM8Fu4TrgkNSmXh5cw/giphy.gif?cid=ecf05e47ulmw29k70cms7qdcbkgplaao3nqfqlfkaeufuag3&rid=giphy.gif&ct=g");
  background-size: cover;
  background-position: center;
  height: 200px;
  width: 90%;
}

.image-container img {
  @apply h-40 w-40 z-10;
}

.image-container span {
  @apply flex justify-evenly transform skew-x-12 w-80 mt-6 italic text-xs
  shadow-inner border-2 border-gray-400 bg-gradient-to-t from-gray-400 to-gray-100;
}

.moves-container {
  @apply px-5;
}

.moves-container span {
  @apply flex flex-row justify-between;
}
.moves-container p {
  @apply text-left text-sm pb-1.5;
}

.globe-bg {
  @apply w-7 h-7 p-1.5 rounded-full;
  filter: contrast(4);
  box-shadow: inset -7px -4px 12px rgba(0, 0, 0, 0.3);
  background-image: linear-gradient(-45deg, rgba(255, 255, 220, 0.3) 0%, transparent 100%);
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
