<template>
  <div>

    <tr v-for="(item, index) in messages" :key="index" :class="index % 2 === 0 ? 'even' : 'odd'">
      <td>{{ item.id }}</td>
      <td>{{ item.parent_id || 0 }}</td>
      <td>{{ item.lifetime_seconds }}</td>
      <td>{{ item.elapsed_lifespan }}</td>
      <td>{{ item.lifecycle }}</td>
      <td>{{ item.number_of_copies }}</td>
      <td>{{ item.generation }}</td>
      <td>{{ item.match_count }}</td>
      <td>{{ item.fitness }}</td>
    </tr>

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
