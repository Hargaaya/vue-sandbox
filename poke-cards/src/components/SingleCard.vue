<template>
  <div class="card-container">
    <header>
      <h3>{{ CardInfo.name }}</h3>
      <p>{{ CardInfo.hp }} hp</p>
      <p>{{ CardInfo.type }}</p>
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
</template>
<script>
export default {
  name: "SingleCard",
  props: {
    CardInfo: Object,
  },
};
</script>
<style>
.card-container {
  @apply bg-green-300 border-8 border-yellow-400 flex flex-col rounded-2xl;
  height: 600px;
  width: 400px;
}

header {
  @apply flex flex-row justify-between m-3;
}

header h3 {
  @apply font-bold;
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
