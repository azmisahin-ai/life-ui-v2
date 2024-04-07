<template>
  <div>

    <div class="chart-container">
      <LineChart :dataList="dataList" idKey="id" valueKey="lifetime_seconds" chartTitle="lifetime"
        lineColor="rgba(75, 192, 192, 1)" />
    </div>

    <div class="chart-container">
      <LineChart :dataList="dataList" idKey="id" valueKey="elapsed_lifespan" chartTitle="lifespan"
        lineColor="rgba(255, 99, 132, 1)" />
    </div>

    <div class="chart-container">
      <LineChart :dataList="dataList" idKey="id" valueKey="match_count" chartTitle="match"
        lineColor="rgba(150, 99, 132, 1)" />
    </div>

    <div class="chart-container">
      <LineChart :dataList="dataList" idKey="id" valueKey="fitness" chartTitle="fitness"
        lineColor="rgba(100, 99, 132, 1)" />
    </div>

  </div>
</template>

<script>
import LineChart from './LineChart.vue';

export default {
  components: {
    LineChart,
  },
  props: {
    dataList: {
      type: Array,
      default: () => [],
    },
  },
};
</script>

<style scoped>
.chart-container {
  display: inline-block;
  width: 50%;
  /* İki çizgi grafiği yatay olarak yan yana gelecek şekilde */
}
</style>
