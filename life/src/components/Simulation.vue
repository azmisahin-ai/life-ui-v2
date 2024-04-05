<template>
  <div class="simulation">
    <div class="panel main">
      <div class="title-overlay">
        <Widget class="title" title="Simulation"></Widget>
      </div>
      <div class="canvas-container">
        <Widget class="canvas" title=""></Widget>
      </div>
      <div class="console-overlay">
        <Widget class="console" title="console"></Widget>
      </div>
      <div class="information-overlay">
        <Widget class="simulastion_status" title="Simulation Status"></Widget>
        <Widget
          class="simulastion_sampler_status"
          title="Sampler Status"
        ></Widget>
        <Widget
          class="simulastion_instance_status"
          title="Instance Status"
        ></Widget>
      </div>
    </div>
    <div class="panel sidebar">
      <div class="toolbar">
        <Widget title="program"></Widget>
        <Widget title="config"></Widget>
        <Widget title="view"></Widget>
        <Widget title="action"></Widget>
        <Widget title="formula"></Widget>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  title: String,
});
</script>

<script>
import Widget from "./Widget.vue";
export default {
  components: {
    Widget,
  },
};
</script>

<style>
/* Reset default margin and padding */
html,
body {
  background-color: rgb(212, 207, 207);
  height: 100%;
  margin: 0;
  padding: 0;
  font-size: 1em;
}

/* Ensure the simulation div fills the entire viewport */
.simulation {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: row;
  /* Arrange panels horizontally */
}

.panel {
 
  display: flex;
  flex-direction: column;
  /* Arrange panel contents vertically */
  flex: 1;
  /* Each panel takes up equal space */
  overflow: hidden;
  /* Prevent content overflow */
}

.main {
  position: relative;
}

.title-overlay {
  position: absolute;
  top: 0;
  right: 30%;
  left: 30%;
  width: 30%;
  /* Width of information panel */
  background-color: rgba(113, 113, 116, 0.242);
  /* Transparent white background */
  padding: 0.2em;
}

.canvas-container {
  flex: 1;
  /* Take remaining vertical space */
  position: relative;
  /* Ensure relative positioning for absolute children */
}

.console-overlay {
  height: 20%;
  /* Set fixed height for the console */
  position: absolute;

  bottom: 0;
  width: 100%;
  /* Width of information panel */
  background-color: rgba(45, 41, 41, 0.538);
  /* Transparent white background */
  padding: 0.2em;
  color: white;
}

.canvas {
  width: 100%;
  height: 100%;
  /* Fill canvas-container height */
}

.information-overlay {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  width: 30%;
  /* Width of information panel */
  background-color: rgba(255, 255, 255, 0.297);
  /* Transparent white background */
  padding: 0.2em;
}

.sidebar {
  flex: 0 0 30%;
  /* Fixed width for sidebar */
  display: flex;
  flex-direction: column;
}

.toolbar {
  flex: 1;
  /* Fill sidebar height */
}
</style>
