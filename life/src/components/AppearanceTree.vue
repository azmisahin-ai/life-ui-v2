<template>
  <div class="tree">
    <h3>AppearanceTree</h3>
    <canvas ref="appearanceTreeCanvas"></canvas>
    <div v-for="(item, index) in messages">
      {{ item.codes }}
    </div>
  </div>
</template>

<script>
export default {
  name: 'AppearanceTree',
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

      this.messages.forEach((data, index) => {
        ctx.fillStyle = 'green';
        ctx.beginPath();
        ctx.arc(50 + index * 50, 50, data.id * 5, 0, Math.PI * 2);
        ctx.fill();
      });
      console.log("AppearanceTree", this.messages)
    },
  },
  watch: {
    data() {
      this.drawCanvas();
    },
  },
};
</script>

<style scoped>
.tree {
  border: 1px solid #ddd;
  padding: 10px;
  border-radius: 5px;
}
</style>