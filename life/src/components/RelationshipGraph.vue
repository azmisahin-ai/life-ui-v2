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
      this.createRelationshipGraph();
    },
    watch: {
      dataList: {
        handler() {
          this.updateNodes();
        },
        deep: true,
      },
    },
    methods: {
      createRelationshipGraph() {
        const self = this;
        this.p5Instance = new p5((p) => {
          p.setup = () => {
            const canvasContainer = self.$refs.canvasContainer;
            const canvasWidth = canvasContainer.clientWidth;
            const canvasHeight = canvasContainer.clientHeight;
            p.createCanvas(canvasWidth, canvasHeight).parent(canvasContainer);
  
            self.updateNodes(); // Düğümleri oluştur ve güncelle
          };
        });
      },
  
      updateNodes() {
        const margin = 40;
        this.nodes = [];
        this.dataList.forEach((node, index) => {
          const x = margin + (index % 10) * 100;
          const y = margin + Math.floor(index / 10) * 100;
          const diameter = 50; // Sabit boyut
          const color = this.getColor(node.id);
  
          this.nodes.push({ id: node.id, x, y, diameter, color, data: node });
        });
  
        this.redrawCanvas();
      },
  
      redrawCanvas() {
        const p = this.p5Instance;
  
        // p.clear();
  
        // Düğüm bağlantılarını çiz
        // p.stroke(255);
        // p.strokeWeight(2);
        this.nodes.forEach((node) => {
          const parent_id = node.data.parent_id;
          if (parent_id) {
            const parentNode = this.nodes.find((n) => n.id === parent_id);
            if (parentNode) {
              p.line(node.x, node.y, parentNode.x, parentNode.y);
            }
          }
  
          // Düğümleri çiz
          p.fill(node.color);
          p.ellipse(node.x, node.y, node.diameter, node.diameter);
          p.fill(0);
          p.text(node.id, node.x, node.y);
        });
      },
  
      getColor(id) {
        if (!this.p5Instance) {
          return '#000000'; // Varsayılan bir renk döndür
        }
  
        const hue = (id * 137) % 360;
        const color = this.p5Instance.color(`hsl(${hue}, 70%, 70%)`);
  
        return color; // Geçerli renk değerini döndür
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
  