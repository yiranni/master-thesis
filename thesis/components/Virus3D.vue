<template>
  <canvas id="virus" ref="virus"></canvas>
</template>

<script>
import * as THREE from "three";
export default {
  name: "Virus3D",
  data() {
    return {
      scene: new THREE.Scene(),
      cam: null,
      renderer: null
    };
  },
  mounted() {
    this.setup();
    this.loop();
    window.addEventListener("resize", () => {
      this.resizeCanvas();
    });
  },
  methods: {
    setup() {
      const canvas = this.$refs.virus;
      this.cam = new THREE.PerspectiveCamera(
        50,
        window.innerWidth / window.innerHeight,
        0.1,
        1000
      );

      this.cam.position.z = 8;

      this.renderer = new THREE.WebGLRenderer({
        canvas: canvas,
        antialias: true,
        alpha: true
      });

      this.renderer.setSize(window.innerWidth, window.innerHeight);

      this.resizeCanvas(window.innerWidth, window.innerHeight);

      this.gemetry = new THREE.IcosahedronBufferGeometry(1, 2);
      this.meshMaterial = new THREE.MeshPhongMaterial({
        color: 0x0a1b2f,
        emissive: 0x072534,
        flatShading: true
      });
      this.lineMaterial = new THREE.LineBasicMaterial({
        color: 0x3ee4ee,
        opacity: 1,
        linewidth: 5
      });
      this.group = new THREE.Group();
      this.group.position.set(0, 0, 0);
      this.mesh = new THREE.Mesh(this.geometry, this.meshMaterial);
      this.line = new THREE.LineSegments(this.geometry, this.lineMaterial);
      this.group.add(this.mesh);
      this.group.add(this.line);
      this.scene.add(this.group);

    },
    loop() {
        this.renderer.render(this.scene, this.cam);
        requestAnimationFrame(this.loop)
    },
    resizeCanvas() {
        let width = window.innerWidth;
        let height = window.innerHeight;
        this.renderer.setSize(width, height);
        this.cam.aspect = width/height;
        this.cam.updateProjectionMatrix()
    }
  }
};
</script>

<style scoped>
canvas {
  background: #0a1728;
  width: 100%;
  height: 100%;
}
</style>