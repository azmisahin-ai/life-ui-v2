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
    };
  },
  mounted() {
    this.createFamilyTree();
  },
  watch: {
    dataList: {
      handler() {
        this.redrawFamilyTree();
      },
      deep: true,
    },
  },
  methods: {
    createFamilyTree() {
      const sketch = (p) => {
        let nodeCoordinates = {}; // nodeCoordinates'i setup içinde tanımla

        p.setup = () => {
          const canvasContainer = this.$refs.canvasContainer;
          const canvasWidth = canvasContainer.clientWidth;
          const canvasHeight = canvasContainer.clientHeight;
          const canvas = p.createCanvas(canvasWidth, canvasHeight);
          canvas.parent(canvasContainer);

          console.log("canvasWidth", canvasWidth)
          console.log("canvasHeight", canvasHeight)
          p.noLoop(); // p5.js draw() döngüsünü devre dışı bırak

          this.determineNodeCoordinates(p, nodeCoordinates); // nodeCoordinates'i determineNodeCoordinates'e geçir
          this.drawTree(p, nodeCoordinates); // nodeCoordinates'i drawTree'ye geçir
        };

        this.determineNodeCoordinates = (p, nodeCoordinates) => {
          this.dataList.forEach(node => {
            const x = p.random(p.width);
            const y = p.random(p.height);
            nodeCoordinates[node.id] = { x, y };
          });
        };

        this.drawTree = (p, nodeCoordinates) => {
          p.background(255, 255, 255, 20); // 100 alpha değeri ile beyaz ve %60 opak arka plan

          p.fill(0);

          // Düğümleri çizme
          for (const id in nodeCoordinates) {
            const { x, y } = nodeCoordinates[id];
            p.ellipse(x, y, 20, 20);
            p.text(id, x - 8, y + 5);
          }

          // Bağlantıları çizme
          p.stroke(0);
          p.strokeWeight(2);
          for (const id in nodeCoordinates) {
            const { x, y } = nodeCoordinates[id];
            const parentNode = this.dataList.find(node => node.id === id);
            if (parentNode && parentNode.parent_id !== 0) {
              const { x: parentX, y: parentY } = nodeCoordinates[parentNode.parent_id];
              p.line(parentX, parentY, x, y);
            }
          }
        };
      };

      // Yeni p5 instance'ını oluştur ve tut
      this.p5Instance = new p5(sketch);
    },

    redrawFamilyTree() {
      if (this.p5Instance) {
        // Önceki p5 instance'ını kaldır
        this.p5Instance.remove();
        // Yeni aile ağacını oluştur
        this.createFamilyTree();
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
