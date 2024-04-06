<template>
  <div ref="canvasContainer" class="canvas-container"></div>
</template>

<script>
import p5 from 'p5';

export default {
  props: {
    dataList: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      p5Instance: null,
      nodes: [], // Düğümleri tutmak için dizi
    };
  },
  mounted() {
    this.createBrainstorm();
  },
  watch: {
    dataList: {
      handler() {
        this.redrawBrainstorm();
      },
      deep: true,
    },
  },
  methods: {
    createBrainstorm() {
      const sketch = (p) => {
        p.setup = () => {
          const canvasContainer = this.$refs.canvasContainer;
          const canvasWidth = canvasContainer.clientWidth;
          const canvasHeight = canvasContainer.clientHeight;
          const canvas = p.createCanvas(canvasWidth, canvasHeight);
          canvas.parent(canvasContainer);
          p.noLoop(); // p5.js draw() döngüsünü devre dışı bırak

          this.createNodes(p); // Düğümleri oluştur
          this.drawBrainstorm(p); // Brainstorm'u çiz
        };
      };

      // Yeni p5 instance'ını oluştur ve tut
      this.p5Instance = new p5(sketch);
    },

    createNodes(p) {
      // Düğümleri oluştur
      const margin = 40;
      const nodeWidth = 80;
      const nodeHeight = 40;
      const startX = margin; // Başlangıç X koordinatı
      const startY = margin; // Başlangıç Y koordinatı

      this.nodes = [];
      this.dataList.forEach((node, index) => {
        const x = startX + (index % 3) * (nodeWidth + margin);
        const y = startY + Math.floor(index / 3) * (nodeHeight + margin);
        const color = this.getColor(node.id); // Düğüm rengini belirle
        this.nodes.push({ id: node.id, parent_id: node.parent_id, x, y, color });
      });
    },

    getColor(id) {
      // Düğüm rengini belirleme fonksiyonu
      const hue = (id * 137) % 360; // ID'ye dayalı renk oluşturma
      return `hsl(${hue}, 70%, 70%)`; // HSL renk formatı kullanarak renk oluştur
    },

    drawBrainstorm(p) {
      p.background(255); // Beyaz arka plan
      p.textAlign(p.CENTER, p.CENTER);
      p.textSize(16);

      // Düğümleri ve bağlantıları çizme
      this.nodes.forEach(({ id, parent_id, x, y, color }) => {
        p.fill(color);
        p.stroke(color);
        p.ellipse(x, y, 40, 40); // Düğüm çizimi
        p.fill(0);
        p.text(id, x, y); // Düğümün üzerine ID yazma

        const parentNode = this.nodes.find(n => n.id === parent_id);
        if (parentNode) {
          const parentX = parentNode.x;
          const parentY = parentNode.y;
          p.line(parentX, parentY, x, y); // Bağlantı çizimi
        }
      });
    },

    redrawBrainstorm() {
      if (this.p5Instance) {
        // Önceki p5 instance'ını kaldır
        this.p5Instance.remove();
        // Yeni brain storm'u oluştur
        this.createBrainstorm();
      }
    },
  },
};
</script>

<style scoped>
.canvas-container {
  position: relative;
  width: 100%;
  height: 100%;
  /* Ekrana tamamen yayılsın */
}
</style>
