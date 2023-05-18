<template>
   <div ref="container"></div>
</template>

<script>
import * as THREE from 'three';

export default {
  name: 'Background1',
  mounted() {
    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);

    const renderer = new THREE.WebGLRenderer();
    renderer.setSize(window.innerWidth, window.innerHeight);
    this.$refs.container.appendChild(renderer.domElement);

    const waterGeometry = new THREE.PlaneGeometry(100, 100, 10, 10);
    const waterTexture = new TextureLoader().load('https://threejs.org/examples/textures/water.jpg');
    waterTexture.wrapS = waterTexture.wrapT = THREE.RepeatWrapping;
    const waterMaterial = new THREE.MeshBasicMaterial({ map: waterTexture, transparent: true, opacity: 0.5 });
    const water = new THREE.Mesh(waterGeometry, waterMaterial);
    water.rotation.x = -Math.PI / 2;
    scene.add(water);

    camera.position.z = 5;

    function animate() {
      requestAnimationFrame(animate);
      water.rotation.z += 0.01;
      renderer.render(scene, camera);
    }

    animate();
  },
}
</script>

<style>

</style>
