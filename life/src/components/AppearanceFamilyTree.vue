<template>
  <div>

    <div v-for="(item, index) in messages" :key="index">
      {{ item.id }}
      {{ item.parent_id || 0 }}
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
