<template>
  <div class="welcome">
    <h1>{{ greeting }}</h1>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const greeting = ref('Hello');

// Simulate a component update to test HMR
setTimeout(() => {
  greeting.value = 'Life';
}, 2000); // Wait 2 seconds to see HMR in action
</script>

<style scoped>
.hello {
  font-family: Arial, sans-serif;
  text-align: center;
}
</style>
