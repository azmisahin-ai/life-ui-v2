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
      nodes: [],
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
      const self = this;
      this.p5Instance = new p5((p) => {
        p.setup = () => {
          const canvasContainer = self.$refs.canvasContainer;
          const canvasWidth = canvasContainer.clientWidth;
          const canvasHeight = canvasContainer.clientHeight;
          p.createCanvas(canvasWidth, canvasHeight).parent(canvasContainer);
          p.noLoop(); // p5.js draw() döngüsünü devre dışı bırak

          self.createNodes(p); // Düğümleri oluştur
          self.drawBrainstorm(p); // Brainstorm'u çiz
        };

        // p.mouseClicked = () => {
        //   self.checkClick(p);
        // };
      });
    },

    createNodes(p) {
      const margin = 40;
      this.nodes = [];
      this.dataList.forEach((node, index) => {
        const x = margin + (index % 10) * 100;
        const y = margin + Math.floor(index / 10) * 100;
        const diameter = node.lifetime_seconds; // Düğümün büyüklüğü lifetime_seconds'a bağlı
        const color = this.getColor(node.id);

        this.nodes.push({ id: node.id, x, y, diameter, color, data: node });
      });
    },

    drawBrainstorm(p) {

      p.textAlign(p.CENTER, p.CENTER);
      p.textSize(16);

      // Düğümleri çiz
      this.nodes.forEach((node) => {
        p.fill(node.color);
        p.stroke(node.color);
        p.ellipse(node.x, node.y, node.diameter, node.diameter);
        p.fill(0);
        p.text(node.id, node.x, node.y);

        // Parent bağlantılarını çiz
        const parent_id = node.data.parent_id;
        if (parent_id) {
          const parentNode = this.nodes.find((n) => n.id === parent_id);
          if (parentNode) {
            // Parent düğümün merkez koordinatları
            const parentX = parentNode.x;
            const parentY = parentNode.y;

            // Düğümün merkez koordinatları
            const nodeX = node.x;
            const nodeY = node.y;

            // Çizgiyi çiz
            p.noFill();
            p.stroke(parentNode.color);
            p.strokeWeight(2);

            // Eğriyi çizmek için yön belirle
            const curveFactor = 0.5; // Eğri faktörü (0 ile 1 arasında)
            const curveX1 = nodeX + (parentX - nodeX) * curveFactor;
            const curveY1 = nodeY + (parentY - nodeY) * curveFactor;
            const curveX2 = parentX - (parentX - nodeX) * curveFactor;
            const curveY2 = parentY - (parentY - nodeY) * curveFactor;

            // Eğriyi çiz
            p.curve(curveX1, curveY1, nodeX, nodeY, parentX, parentY, curveX2, curveY2);
          }
        }
      });
    },


    redrawBrainstorm() {
      if (this.p5Instance) {
        this.p5Instance.remove();
        this.createBrainstorm();
      }
    },

    getColor(id) {
      if (!this.p5Instance) {
        return '#000000'; // Varsayılan bir renk döndür
      }

      const hue = (id * 137) % 360;
      const color = this.p5Instance.color(`hsl(${hue}, 70%, 70%)`);

      return color; // Geçerli renk değerini döndür
    },



    checkClick(p) {
      this.nodes.forEach((node) => {
        const distance = p.dist(p.mouseX, p.mouseY, node.x, node.y);
        if (distance < node.diameter / 2) {
          this.showNodeInfo(node);
        }
      });
    },

    showNodeInfo(node) {
      const info = `
        ID: ${node.id}
        Parent ID: ${node.data.parent_id}
        Lifetime Seconds: ${node.data.lifetime_seconds}
        Life Created Time: ${node.data.life_created_time}
        Life Start Time: ${node.data.life_start_time}
        Elapsed Lifespan: ${node.data.elapsed_lifespan}
        Lifecycle: ${node.data.lifecycle}
        Life Status: ${node.data.life_status}
        Codes: ${node.data.codes.join(', ')}
        Number of Copies: ${node.data.number_of_copies}
        Generation: ${node.data.generation}
        Match Count: ${node.data.match_count}
        Fitness: ${node.data.fitness}
      `;
      alert(info);
    },
  },
};
</script>

<style scoped>
.canvas-container {
  position: relative;
  width: 100%;
  height: 100%;
}
</style>
