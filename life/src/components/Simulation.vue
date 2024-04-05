<template>
  <div class="simulation">
    <panel class="panel main">
      <div class="canvas-container">
        <div class="title-overlay">
          <Widget class="title" title="Simulation"></Widget>
        </div>
        <Widget class="canvas" title=""></Widget>
      </div>
      <div class="console-overlay">
        <Widget class="console" title="Console"></Widget>
      </div>
      <div class="information-overlay">
        <Widget class="simulation-status" title="Simulation Status"></Widget>
        <Widget class="sampler-status" title="Sampler Status"></Widget>
        <Widget class="instance-status" title="Instance Status"></Widget>
      </div>
    </panel>

    <panel class="panel sidebar">
      <toolbar class="toolbar">
        <div class="widget socket">
          <label>Program Address</label>
          <input name="program_address" type="url" placeholder="http://">
          <button name="connect">Connect</button>
        </div>

        <div class="widget simulations">
          <div class="tab core">
            <label>Number of Instances</label>
            <input name="number_of_instance" type="range" min="1" max="100" step="1">
            <label>Lifetime</label>
            <input name="lifetime_seconds" type="range" min="0.1" max="60.0" step="0.1">
            <label>Number of Replicas</label>
            <input name="number_of_replicas" type="range" min="1" max="10" step="1">
            <label>Number of Generations</label>
            <input name="number_of_generation" type="range" min="1" max="10" step="1">
            <label>Maximum Match Limit</label>
            <input name="max_match_limit" type="range" min="1" max="10" step="1">
          </div>
          <div class="tab particles"></div>
        </div>

        <div class="widget appearance">
          <label>Appearance</label>
          <select name="appearance">
            <option selected>Simulation</option>
            <option>Directory Tree</option>
            <option>Family Tree</option>
          </select>
        </div>

        <div class="widget action">
          <button name="start">Start</button>
          <button name="pause">Pause</button>
          <button name="resume">Resume</button>
          <button name="stop">Stop</button>
        </div>

        <div class="widget formula">
          <label>Formula</label>
          <textarea name="formula" placeholder="5.5 * self.generation"></textarea>
          <button name="apply">Apply</button>
        </div>
      </toolbar>
    </panel>
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
  height: 100%;
  margin: 0;
  padding: 0;
  font-size: 16px;
}

/* Main simulation container */
.simulation {
  display: flex;
  height: 100%;
}

/* Panels */
.panel {
  flex: 1;
  display: flex;
  flex-direction: column;
  padding: 10px;
}

/* Main panel */
.main {
  position: relative;
  background-color: #f5f5f5;
  border: 1px solid #ddd;
  border-radius: 5px;
  margin-right: 10px;
  flex: 1;
  /* Main panel takes up remaining space */
}

.canvas-container {
  flex: 1;
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.title-overlay {
  background-color: #ccc;
  padding: 10px;
  border-radius: 5px;
}

.canvas {
  width: 100%;
  height: 100%;
}

.console-overlay {
  background-color: #333;
  color: white;
  padding: 10px;
  flex: 0.25;
  /* Console takes 25% of main panel height */
  overflow-y: auto;
  /* Enable vertical scrolling if needed */
}

.information-overlay {
  position: absolute;
  top: 10px;
  right: 10px;
  width: 25%;
  background-color: rgba(255, 255, 255, 0.8);
  padding: 10px;
  border-radius: 5px;
}

/* Sidebar panel */
.sidebar {
  flex: 0 0 25%;
  /* Sidebar takes up 25% of simulation width */
  max-width: 25%;
  /* Max width to prevent exceeding 25% */
  background-color: #f5f5f5;
  border: 1px solid #ddd;
  border-radius: 5px;
  overflow-y: auto;
  /* Enable vertical scrolling if needed */
}


.toolbar {
  padding: 10px;
}

.widget {
  margin-bottom: 20px;
}

.widget label {
  display: block;
  margin-bottom: 5px;
}

.widget input,
.widget select,
.widget textarea {
  width: 100%;
  padding: 5px;
  border: 1px solid #ddd;
  border-radius: 3px;
}

.widget button {
  padding: 8px 16px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 3px;
  cursor: pointer;
}

.widget button:hover {
  background-color: #0056b3;
}
</style>
