<template>
  <div class="appearance-simulation">
    <h3>Simulation Data</h3>
    <canvas ref="simulationCanvas"></canvas>
  </div>
</template>

<script>
export default {
  name: 'AppearanceSimulation',
  props: {
    dataList: {
      type: Array,
      default: () => [],
    },
  },
  mounted() {
    this.drawCanvas();
  },
  methods: {
    drawCanvas() {
      const canvas = this.$refs.simulationCanvas;
      const ctx = canvas.getContext('2d');

      // Clear canvas
      ctx.clearRect(0, 0, canvas.width, canvas.height);

      this.dataList.forEach((data, index) => {
        ctx.fillStyle = 'green';
        ctx.beginPath();
        ctx.arc(50 + index * 50, 50, data.id * 5, 0, Math.PI * 2);
        ctx.fill();
      });
      console.log("familyTreeCanvas", this.dataList)
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
.appearance-family-tree {
  border: 1px solid #ddd;
  padding: 10px;
  border-radius: 5px;
}
</style>