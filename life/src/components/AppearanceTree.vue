<template>
  <div class="tree">


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
};
</script>

<style scoped>
.tree {
  border: 1px solid #ddd;
  padding: 10px;
  border-radius: 5px;
}
</style>