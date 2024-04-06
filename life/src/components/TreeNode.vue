<template>
  <g>
    <!-- Düğümü çiz -->
    <circle :cx="x" :cy="y" :r="radius" fill="#69b3a2" stroke="black" stroke-width="2" />
    <!-- Düğümün bilgilerini yaz -->
    <text :x="x" :y="y - radius - 5" font-size="12" text-anchor="middle" alignment-baseline="middle">
      {{ node.id }}
    </text>
    <!-- Düğümün bağlantılarını çiz -->
    <line v-for="child in children" :key="child.id" :x1="x" :y1="y + radius" :x2="childX(child)" :y2="childY"
      stroke="black" stroke-width="2" />
    <!-- Çocuk düğümleri için alt düğümleri oluştur -->
    <TreeNode v-for="child in children" :key="child.id" :node="child" :dataList="dataList" :x="childX(child)"
      :y="childY" :depth="depth + 1" />
  </g>
</template>

<script>
export default {
  props: {
    node: {
      type: Object,
      required: true,
    },
    dataList: {
      type: Array,
      default: () => [],
    },
    x: {
      type: Number,
      default: 0,
    },
    y: {
      type: Number,
      default: 0,
    },
    depth: {
      type: Number,
      default: 0,
    },
  },
  computed: {
    children() {
      // Bu düğümün çocuk düğümlerini filtrele
      return this.dataList.filter(item => item.parent_id === this.node.id);
    },
    radius() {
      // Düğüm yarıçapını hesapla
      return 20;
    },
    childX() {
      // Çocuk düğümlerin x konumunu hesapla
      return (child) => {
        return this.x + (this.radius + 20) * (this.children.indexOf(child) - (this.children.length - 1) / 2);
      };
    },
    childY() {
      // Çocuk düğümlerin y konumunu hesapla
      return this.y + 60;
    },
  },
};
</script>
