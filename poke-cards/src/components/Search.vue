<template>
  <div class="search-container">
    <h3>Search</h3>
    <input type="text" placeholder="ex. Blastoise" @change="$emit('onSearch', $event.target.value)" />
  </div>
</template>
<script>
export default {
  name: "SearchMultiWords",
};
</script>
<style scoped>
.search-container {
  @apply bg-gray-50 rounded-2xl w-96 h-24 border-2 border-red-400
    mx-auto text-center font-bold text-gray-400 shadow-sm
    flex flex-col items-center justify-center;
}

input {
  @apply w-72 rounded-xl p-2 m-2 outline-none bg-transparent text-black
   shadow-inner;
}
</style>
