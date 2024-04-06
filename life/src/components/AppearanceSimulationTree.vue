<template>
  <div class="appearance-simulation-tree">
    <h3>Simulation Tree</h3>
    <canvas ref="simulationTreeCanvas"></canvas>
    <div v-for="(item, index) in messages">
      {{ item.id }}
    </div>
  </div>
</template>

<script>
export default {
  name: 'AppearanceSimulationTree',
  props: {
    dataList: {
      type: Array,
      default: () => [],
    },
  },
  computed: {
    messages() {
      return this.dataList || [];
    },
  },
  mounted() {
    this.drawCanvas();
  },
  methods: {
    drawCanvas() {
      const canvas = this.$refs.simulationTreeCanvas;
      const ctx = canvas.getContext('2d');

      // Clear canvas
      ctx.clearRect(0, 0, canvas.width, canvas.height);

      this.dataList.forEach((data, index) => {
        ctx.fillStyle = 'green';
        ctx.beginPath();
        ctx.arc(50 + index * 50, 50, data.id * 5, 0, Math.PI * 2);
        ctx.fill();
      });
      console.log("simulationTreeCanvas", this.dataList)
    },
  },
  watch: {
    dataList() {
      this.drawCanvas();
    },
  },
};
</script>

<style scoped>
.appearance-simulation-tree {
  border: 1px solid #ddd;
  padding: 10px;
  border-radius: 5px;
}
</style>