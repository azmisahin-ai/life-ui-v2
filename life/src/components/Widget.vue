<script setup>
const { title, data } = defineProps({
  title: String,
  data: Array,
});
</script>
<template>
  <div class="widget">

    <h4>{{ title }}</h4>
    <div>{{ data }}</div>
  </div>
</template>

<script>

export default {
  // Bileşen mantığı burada
}
</script>

<style scoped>
.widget {
  text-align: left;
  margin: 0.1em;
  padding: 0.1em;
}

.widget h4 {
  margin: 0;
  padding: 0;
}
</style>