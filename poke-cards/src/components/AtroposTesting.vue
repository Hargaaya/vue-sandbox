<template>
  <div>
    <atropos class="my-atropos">
      <img
        src="https://st.depositphotos.com/1020324/1687/i/600/depositphotos_16870977-stock-photo-trees-in-a-forrest.jpg"
        alt="whit"
        data-atropos-offset="-5"
      />
      <h1 data-atropos-offset="5">HELLO!</h1>
    </atropos>
  </div>
</template>
<script>
import Atropos from "atropos/vue";
export default {
  components: { Atropos },
};
</script>
<style>
.my-atropos {
  @apply h-96 w-96 absolute;
}

.my-atropos img {
  @apply w-96 h-96 relative;
}

.my-atropos h1 {
  @apply relative z-10;
}
</style>
