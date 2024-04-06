<template>
  <div>
    <!-- İlk çizgi grafiği -->
    <div class="chart-container">
      <LineChart :dataList="dataList" chartTitle="Lifetime" lineColor="rgba(75, 192, 192, 1)" />
    </div>

    <!-- İkinci çizgi grafiği -->
    <div class="chart-container">
      <LineChart :dataList="dataList" chartTitle="Lifespan" lineColor="rgba(255, 99, 132, 1)" />
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
