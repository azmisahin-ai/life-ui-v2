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
    console.log("mounted", this.dataList)
  },
  methods: {
    drawCanvas() {
      const canvas = this.$refs.simulationCanvas;
      const ctx = canvas.getContext('2d');

      // Clear canvas
      ctx.clearRect(0, 0, canvas.width, canvas.height);

      this.dataList.forEach((data, index) => {

        example_data = {
          name: 'life.core',
          id: 56,
          parent_id: 0,
          lifetime_seconds: 37.38999999999999,
          life_created_time: 1712370315.8409548,
          life_start_time: 1712370315.8421602,
          elapsed_lifespan: 4.09339451789856,
          lifecycle: 0.1,
          life_status: 'Running',
          codes: [
            235, 166, 130, 177, 23, 140, 127, 239,
            223, 234, 67, 203, 154, 240, 225, 42,
            192, 135, 238, 71, 30, 92, 251, 205,
            136, 6, 33, 186, 250, 168, 200, 30,
            124, 237, 90, 248, 195, 183, 41, 168
          ],
          number_of_copies: 0,
          generation: 1,
          match_count: 0,
          fitness: 9.453034597765814
        }

        ctx.fillStyle = 'green';
        ctx.beginPath();
        ctx.arc(50 + index * 50, 50, data.id * 5, 0, Math.PI * 2);
        ctx.fill();
      });
      console.log("simulationCanvas", this.dataList)
    },
  },
  watch: {
    dataList() {
      console.log("simulationCanvas change", this.dataList)
      this.drawCanvas();
    },
  },
};
</script>

<style scoped>
.appearance-simulation {
  border: 1px solid #ddd;
  padding: 10px;
  border-radius: 5px;
}
</style>