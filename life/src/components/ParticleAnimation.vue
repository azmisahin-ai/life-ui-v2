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
        particles: [],
      };
    },
    mounted() {
      this.createParticleAnimation();
    },
    watch: {
      dataList: {
        handler() {
          this.updateParticles();
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
          };
  
          p.draw = () => {
            p.background(255);
            self.updateParticles();
            self.drawParticles();
          };
        });
      },
  
      updateParticles() {
        this.particles = [];
        this.dataList.forEach((data) => {
          const scaledPosition = this.scalePosition(data.position);
          const scaledVelocity = this.scaleVelocity(data.velocity);
  
          this.particles.push({
            x: scaledPosition.x,
            y: scaledPosition.y,
            velocityX: scaledVelocity.x,
            velocityY: scaledVelocity.y,
            diameter: 20,
            color: this.getColor(data.id),
          });
        });
      },
  
      drawParticles() {
        const p = this.p5Instance;
        this.particles.forEach((particle) => {
          p.fill(particle.color);
          p.noStroke();
          p.ellipse(particle.x, particle.y, particle.diameter, particle.diameter);
  
          // Parçacıkları hareket ettir
          particle.x += particle.velocityX;
          particle.y += particle.velocityY;
  
          // Kenarlardan sekme kontrolü
          if (particle.x < 0 || particle.x > p.width) {
            particle.velocityX *= -1;
          }
          if (particle.y < 0 || particle.y > p.height) {
            particle.velocityY *= -1;
          }
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
  
      scalePosition(position) {
        // Veri konumunu uygun şekilde ölçeklendir
        return {
          x: this.mapValue(position.x, -1e27, 1e27, 0, this.p5Instance.width),
          y: this.mapValue(position.y, -1e27, 1e27, 0, this.p5Instance.height),
        };
      },
  
      scaleVelocity(velocity) {
        // Veri hızını uygun şekilde ölçeklendir
        return {
          x: this.mapValue(velocity.x, -1e27, 1e27, -10, 10),
          y: this.mapValue(velocity.y, -1e27, 1e27, -10, 10),
        };
      },
  
      mapValue(value, minInput, maxInput, minOutput, maxOutput) {
        // Değerleri aralıklar arasında dönüştür
        return ((value - minInput) * (maxOutput - minOutput)) / (maxInput - minInput) + minOutput;
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
  