<template>
  <div>
    DirectoryTree
    <canvas ref="canvasTree"></canvas>
    <div v-for="(item, index) in messages">
      {{ item.parentId || 0 }} - {{ item.id }} - {{ item.elapsed_lifespan }} 
    </div>
  </div>
</template>

<script>
export default {
 
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

};
</script>

<style scoped></style>