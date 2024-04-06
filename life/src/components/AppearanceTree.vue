<template>
  <div class="tree">

    <div v-if="appearance === 'DirectoryTree'">
      <AppearanceDirectoryTree :dataList="messages"></AppearanceDirectoryTree>
    </div>


    <div v-else-if="appearance === 'FamilyTree'">
      <AppearanceFamilyTree :dataList="messages"></AppearanceFamilyTree>
    </div>
  </div>
</template>

<script>

import AppearanceDirectoryTree from './AppearanceDirectoryTree.vue';
import AppearanceFamilyTree from './AppearanceFamilyTree.vue';

export default {

  components: {
    AppearanceDirectoryTree, AppearanceFamilyTree
  },
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

<style scoped></style>