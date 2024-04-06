<template>
  <div class="tree">

    <div v-if="appearance === 'DirectoryTree'" class="container">
      <AppearanceDirectoryTree :dataList="messages"></AppearanceDirectoryTree>
    </div>


    <div v-else-if="appearance === 'FamilyTree'" class="container">
      <AppearanceFamilyTree :dataList="messages"></AppearanceFamilyTree>
    </div>

    <div v-else-if="appearance === 'AppearanceStatistic'" class="container">
      <AppearanceStatistic :dataList="messages"></AppearanceStatistic>
    </div>

  </div>


</template>

<script>

import AppearanceDirectoryTree from './AppearanceDirectoryTree.vue';
import AppearanceFamilyTree from './AppearanceFamilyTree.vue';
import AppearanceStatistic from './AppearanceStatistic.vue';
export default {

  components: {
    AppearanceDirectoryTree, AppearanceFamilyTree, AppearanceStatistic
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

<style scoped>
.tree {
  width: 100%;
  height: 100%;
}

.container {
  width: 100%;
  height: 100%;
}
</style>