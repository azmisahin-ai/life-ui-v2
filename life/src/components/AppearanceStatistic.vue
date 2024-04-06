<template>
  <div>
    <!-- Grafik için canvas elementi -->
    <canvas ref="lineChartCanvas"></canvas>
  </div>
</template>

<script>
import { ref, watch, onMounted } from 'vue';
import { Chart, registerables } from 'chart.js';

export default {
  props: {
    dataList: {
      type: Array,
      default: () => [],
    },
  },
  mounted() {
    this.createChart();
  },
  watch: {
    dataList: {
      handler() {
        this.redrawChart();
      },
      deep: true,
    },
  },
  methods: {
    createChart() {
      const lineChartCanvas = this.$refs.lineChartCanvas;
      const ctx = lineChartCanvas.getContext('2d');

      // Chart.js kurulumu
      Chart.register(...registerables);

      // Çizgi grafiği oluştur
      this.lineChart = new Chart(ctx, {
        type: 'line',
        data: {
          labels: this.dataList.map(item => item.id),
          datasets: [
            {
              label: 'Lifetime Seconds',
              data: this.dataList.map(item => item.lifetime_seconds),
              borderColor: 'rgba(75, 192, 192, 1)',
              borderWidth: 2,
              fill: false,
            },
          ],
        },
        options: {
          scales: {
            y: {
              beginAtZero: true,
            },
          },
        },
      });
    },
    redrawChart() {
      if (this.lineChart) {
        this.lineChart.data.labels = this.dataList.map(item => item.id);
        this.lineChart.data.datasets[0].data = this.dataList.map(item => item.lifetime_seconds);
        this.lineChart.update();
      }
    },
  },
};
</script>

<style scoped>
/* İsteğe bağlı stil tanımları */
</style>
