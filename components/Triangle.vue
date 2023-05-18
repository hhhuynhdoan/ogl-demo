<template>
  <div ref="canvasContainer"></div>
</template>

<script>
import { Renderer, Program, Color, Mesh, Triangle } from '../utils/index.js';

export default {
  name: 'Triangle',
  mounted() {
    const vertex = /* glsl */ `
      attribute vec2 uv;
      attribute vec2 position;

      varying vec2 vUv;

      void main() {
          vUv = uv;
          gl_Position = vec4(position, 0, 1);
      }
    `;

    const fragment = /* glsl */ `
      precision highp float;

      uniform float uTime;
      uniform vec3 uColor;

      varying vec2 vUv;

      void main() {
          gl_FragColor.rgb = 0.5 + 0.3 * cos(vUv.xyx + uTime) + uColor;
          gl_FragColor.a = 1.0;
      }
    `;

    const renderer = new Renderer();
    const gl = renderer.gl;
    this.$refs.canvasContainer.appendChild(gl.canvas);
    gl.clearColor(1, 1, 1, 1);

    function resize() {
      renderer.setSize(window.innerWidth, window.innerHeight);
    }
    window.addEventListener('resize', resize, false);
    resize();

    const geometry = new Triangle(gl);

    const program = new Program(gl, {
      vertex,
      fragment,
      uniforms: {
        uTime: { value: 0 },
        uColor: { value: new Color(0.3, 0.2, 0.5) },
      },
    });

    const mesh = new Mesh(gl, { geometry, program });

    requestAnimationFrame(update);
    function update(t) {
      requestAnimationFrame(update);

      program.uniforms.uTime.value = t * 0.001;

      renderer.render({ scene: mesh });
    }
  }
};
</script>

<style>
div {
  width: 100%;
  height: 100%;
  position: absolute;
  z-index: 1;
}
</style>