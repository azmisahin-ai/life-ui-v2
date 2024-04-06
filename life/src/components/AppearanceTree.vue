<template>
  <div class="tree">
    {{ appearance }}

  </div>
</template>

<script>
export default {
  name: 'AppearanceTree',
  props: {
    appearance: {
      type: String,
      default: () => "DirectoryTree"
    },
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