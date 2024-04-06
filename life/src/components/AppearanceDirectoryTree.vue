<template>
  <div class="appearance-directory-tree">
    <h3>Directory Tree Data</h3>
    <canvas ref="directoryTreeCanvas"></canvas>
  </div>
</template>

<script>
export default {
  name: 'AppearanceDirectoryTree',
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
      const canvas = this.$refs.directoryTreeCanvas;
      const ctx = canvas.getContext('2d');

      // Clear canvas
      ctx.clearRect(0, 0, canvas.width, canvas.height);

      this.dataList.forEach((data, index) => {
        ctx.fillStyle = 'green';
        ctx.beginPath();
        ctx.arc(50 + index * 50, 50, data.id * 5, 0, Math.PI * 2);
        ctx.fill();
      });
      console.log("directoryTreeCanvas",this.dataList)
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
.appearance-directory-tree {
  border: 1px solid #ddd;
  padding: 10px;
  border-radius: 5px;
}
</style>