<template>

  <div v-for="(item, index) in messages" :key="index">
    {{ item.id }}
    {{ item.parent_id || 0 }}
    {{ item.lifetime_seconds }}
    {{ item.elapsed_lifespan }}
    {{ item.lifecycle }}
    {{ item.number_of_copies }}
    {{ item.generation }}
    {{ item.match_count }}
    {{ item.fitness }}
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

<style scoped>
table {
  width: 100%;
  border-collapse: collapse;
}

th,
td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}

th {
  background-color: #f2f2f2;
}

.even {
  background-color: #f9f9f9;
  /* Çift satırlar için arka plan rengi */
}

.odd {
  background-color: #ffffff;
  /* Tek satırlar için arka plan rengi */
}
</style>
