<script setup>

</script>
<template>
  <section class="simulation">
    <panel class="main">
      <Widget title="canvas"></Widget>
      <Widget title="information"></Widget>
      <Widget title="console"></Widget>
    </panel>
    <panel class="toolbar">
      <Widget title="program"></Widget>
      <Widget title="config"></Widget>
      <Widget title="view"></Widget>
      <Widget title="action"></Widget>
      <Widget title="formula"></Widget>
    </panel>

  </section>
</template>

<script>
import Widget from './Widget.vue'
export default {
  components: {
    Widget
  }
}
</script>

<style scoped>
.simulation {
  color: gray;
}

.main {

  width: 80%;
  float: left;
}

.toolbar {
  width: 20%;
  float: left;
}
</style>
