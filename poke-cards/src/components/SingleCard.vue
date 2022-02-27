<template>
  <atropos class="my-atropos">
    <div class="card-container">
      <header>
        <h3>{{ CardInfo.name }}</h3>
        <span>
          <p><bold>HP </bold>{{ CardInfo.hp }}</p>
          <i class="fa fa-tree"></i>
        </span>
      </header>

      <div class="card-body">
        <div class="image-container">
          <img :src="CardInfo.image" :alt="CardInfo.name" />
          <span>
            <p>NO: {{ CardInfo.number }}</p>
            <p>Height: {{ CardInfo.height }}</p>
            <p>Weight: {{ CardInfo.weight }}</p>
          </span>
        </div>

        <span class="attacks-container">
          <h2>Attacks</h2>
          <p>{{ CardInfo.attacks }}</p>
        </span>
      </div>
    </div>
  </atropos>
</template>
<script>
import Atropos from "atropos/vue";

export default {
  name: "SingleCard",
  props: {
    CardInfo: Object,
  },
  components: { Atropos },
};
</script>

<style scoped>
.my-atropos {
  height: 600px;
  width: 400px;
}

.card-container {
  @apply bg-green-300 border-8 border-yellow-400 flex flex-col rounded-2xl
  select-none w-full h-full;
}

header {
  @apply flex flex-row justify-between items-center h-10 mt-1 mx-5 font-bold;
}

header span {
  @apply flex w-24 justify-around items-center;
}

header i {
  @apply w-8 h-8 p-1.5 rounded-full shadow-lg bg-green-200;
}

header p > * {
  @apply font-bold text-xs;
}

.image-container {
  @apply border-4 border-gray-400 mx-auto mt-5 mb-10 flex flex-col justify-center items-center;
  background-image: url("https://efi.int/sites/default/files/2020-12/placeholder.jpeg");
  background-size: cover;
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
</style>
