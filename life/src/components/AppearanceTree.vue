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

    <div v-else-if="appearance === 'AppearanceParticle'" class="container">
      <AppearanceParticle :dataList="messages"></AppearanceParticle>
    </div>

  </div>


</template>

<script>

import AppearanceDirectoryTree from './AppearanceDirectoryTree.vue';
import AppearanceFamilyTree from './AppearanceFamilyTree.vue';
import AppearanceStatistic from './AppearanceStatistic.vue';
import AppearanceParticle from './AppearanceParticle.vue';
export default {

  components: {
    AppearanceDirectoryTree, AppearanceFamilyTree, AppearanceStatistic, AppearanceParticle
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