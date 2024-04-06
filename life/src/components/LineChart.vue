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
    chartTitle: {
      type: String,
      default: 'Chart Title',
    },
    lineColor: {
      type: String,
      default: 'rgba(75, 192, 192, 1)',
    },
    valueKey: {
      type: String,
      required: true,
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

      // Veri hazırlığı
      const labels = this.dataList.map(item => `Id: ${item.id}`); // Id'leri etiketlere ekliyoruz
      const data = this.dataList.map(item => item[this.valueKey]); // Verileri çekiyoruz (valueKey'e göre)

      // Çizgi grafiği oluştur
      this.lineChart = new Chart(ctx, {
        type: 'line',
        data: {
          labels: labels,
          datasets: [
            {
              label: this.chartTitle,
              data: data,
              borderColor: this.lineColor,
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
        const labels = this.dataList.map(item => `Id: ${item.id}`);
        const data = this.dataList.map(item => item[this.valueKey]);

        this.lineChart.data.labels = labels;
        this.lineChart.data.datasets[0].data = data;
        this.lineChart.update();
      }
    },
  },
};
</script>

<style scoped>
/* İsteğe bağlı stil tanımları */
</style>
