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
      this.createParticleAnimation();
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
      createParticleAnimation() {
        const self = this;
        this.p5Instance = new p5((p) => {
          p.setup = () => {
            const canvasContainer = self.$refs.canvasContainer;
            const canvasWidth = canvasContainer.clientWidth;
            const canvasHeight = canvasContainer.clientHeight;
            p.createCanvas(canvasWidth, canvasHeight).parent(canvasContainer);
            p.noLoop(); // p5.js draw() döngüsünü devre dışı bırak
  
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
          const diameter = node.lifetime_seconds; // Düğümün büyüklüğü lifetime_seconds'a bağlı
          const color = this.getColor(node.id);
  
          this.nodes.push({ id: node.id, x, y, diameter, color, data: node });
        });
  
        this.redrawCanvas();
      },
  
      redrawCanvas() {
        const p = this.p5Instance;
  
        // p.clear();
  
        // Düğümleri çiz
        this.nodes.forEach((node) => {
          p.fill(node.color);
          p.stroke(node.color);
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
  