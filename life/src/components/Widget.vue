<script setup>
defineProps({
  title: String,
});
</script>
<template>
  <div class="widget">

    <h4>{{ title }}</h4>

  </div>
</template>

<script>

export default {
  // Bileşen mantığı burada
}
</script>

<style scoped>
.widget {
  color: darkgrey;
  text-align: left;

}
</style>