<script setup lang="ts">
import TheGallery from "../src/components/TheGallery.vue"

// export default defineComponent({
//   name: 'IndexPage',

//   components: {
//     TheGallery,
//   },
// });
</script>

<template>
  <main>
    <h1>Mockups</h1>
    <TheGallery />
  </main>
</template>

<style lang="scss" scoped>

  main {
    max-width: 1366px;
    padding: 15px;
    margin: 30px auto;
  }
  
</style>
