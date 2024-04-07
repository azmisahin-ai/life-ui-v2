<template>
  <div>
    <table>
      <thead>
        <tr>
          <th>ID</th>
          <th>ParentID</th>
          <th>Lifetime</th>
          <th>Lifespan</th>
          <th>LifeCycle</th>
          <th>Copies</th>
          <th>Generation</th>
          <th>Match</th>
          <th>Fitness</th>
        </tr>
      </thead>
      <tbody>
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
      </tbody>
    </table>
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
