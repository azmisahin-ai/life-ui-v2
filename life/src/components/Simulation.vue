<template>
  <div class="simulation">

    <panel class="panel main">

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
        <Widget class="simulastion_sampler_status" title="Sampler Status"></Widget>
        <Widget class="simulastion_instance_status" title="Instance Status"></Widget>
      </div>

    </panel>

    <panel class="panel sidebar">

      <toolbar class="toolbar">

        <div class="widget socket">
          <div class="input-group">
            <label>Program Address</label>
            <input name="program_address" type="url" placeholder="http://"> </input>
          </div>
          <div class="input-group">
            <button name="connect">Connect</button>
          </div>
        </div>

        <div class="widget simulations">

          <div class="tab core">
            <div class="input-group">
              <label>Number of instance</label>
              <input name="number_of_instance" type="range" min="1" max="100" step="1"> </input>
            </div>
            <div class="input-group">
              <label>Lifetime</label>
              <input name="lifetime_seconds" type="range" min="0.1" max="60.0" step="0.1"> </input>
            </div>
            <div class="input-group">
              <label>Number of replicas</label>
              <input name="number_of_replicas" type="range" min="1" max="10" step="1"> </input>
            </div>
            <div class="input-group">
              <label>Number of generation</label>
              <input name="number_of_generation" type="range" min="1" max="10" step="1"> </input>
            </div>
            <div class="input-group">
              <label>Maximum Match Limit</label>
              <input name="max_match_limit" type="range" min="1" max="10" step="1"> </input>
            </div>
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
          <div class="input-group">
            <button name="start">Start</button>
            <button name="pause">Pause</button>
            <button name="resume">Resume</button>
            <button name="stop">Stop</button>
          </div>
        </div>

        <div class="widget formula">
          <div class="input-group">
            <label>Formula</label>
            <textarea name="formula" placeholder="5.5 * self.generation"> </textarea>
          </div>
          <div class="input-group">
            <button name="apply">Apply</button>
          </div>
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
  width: 25%;
  /* Width of information panel */
  background-color: rgba(255, 255, 255, 0.297);
  /* Transparent white background */
  padding: 0.2em;
}

.sidebar {
  flex: 0 0 25%;
  /* Fixed width for sidebar */
  display: flex;
  flex-direction: column;
}

.toolbar {
  flex: 1;
  /* Fill sidebar height */
}
</style>
