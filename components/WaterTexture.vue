<template>
  <div ref="container"></div>
</template>

<script>
import { WaterTexture } from '../utils/WaterTexture.js';

export default {
  name: 'WaterTexture',
  mounted() {
    this.init();
  },
  methods: {
    init() {
      this.waterTexture = new WaterTexture({ debug: true });

      window.addEventListener('mousemove', this.onMouseMove.bind(this));
      this.tick();
    },
    onMouseMove(ev) {
      const point = {
        x: ev.clientX / window.innerWidth,
        y: ev.clientY / window.innerHeight,
      };

      this.waterTexture.addPoint(point);
    },
    tick() {
      this.waterTexture.update();
      requestAnimationFrame(this.tick);
    },
  },
};
</script>

<style>
div {
  width: 100%;
  height: 100%;
  position: absolute;
  z-index: 0;
}
</style>