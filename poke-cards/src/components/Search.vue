<template>
  <div class="search-container">
    <input type="text" placeholder="ex. blastoise or 9" @change="$emit('onSearch', $event.target.value)" />
  </div>
</template>
<script>
export default {
  name: "SearchMultiWords",
};
</script>
<style scoped>
.search-container {
  @apply bg-gray-50 rounded-2xl w-96 h-24 border-8 border-red-300
    mx-auto text-center font-bold text-gray-500 shadow-sm
    flex flex-col items-center justify-center;
}

input {
  @apply w-72 rounded-xl p-2 m-2 outline-none bg-red-200 text-black
   shadow-inner;
}
</style>
