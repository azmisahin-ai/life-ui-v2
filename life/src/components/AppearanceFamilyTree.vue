<template>
  <svg width="800" height="600">
    <TreeNode v-for="rootNode in rootNodes" :key="rootNode.id" :node="rootNode" :dataList="dataList" :x="400" :y="50" />
  </svg>
</template>

<script>
import TreeNode from "./TreeNode.vue";

export default {
  props: {
    dataList: {
      type: Array,
      default: () => [],
    },
  },
  computed: {
    rootNodes() {
      // Kök düğümleri filtrele
      return this.dataList.filter(item => !item.parent_id);
    },
  },
  components: {
    TreeNode,
  },
};
</script>
