<template>
  <div>
    <canvas id="virus" ref="virus"></canvas>
    <div class="tooltip">
      <h1 class="virus-name">{{virus}}</h1>
    </div>
  </div>
</template>

<script>
import {
  Scene,
  PerspectiveCamera,
  WebGLRenderer,
  IcosahedronBufferGeometry,
  CylinderGeometry,
  MeshPhongMaterial,
  LineBasicMaterial,
  Group,
  Mesh,
  LineSegments,
  Color,
  PointLight,
  Raycaster,
  Vector2
} from "three";

// import { OrbitControls } from "three-orbit-controls";
const THREE = require("three");
const OrbitControls = require("three-orbit-controls")(THREE);

import VirusData from "~/assets/virus-grouped.json";
import $ from "jquery";

export default {
  name: "Virus3D",
  data() {
    return {
      VirusData: VirusData,
      scene: new Scene(),
      cam: null,
      renderer: null,
      control: null,
      raycaster: null,
      mouse: null,
      intersects: null,
      virus: null
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
      const nodeCorr = [
        [1, 0, 0],
        [0.31, -0.95, 0],
        [0.31, 0.95, 0],
        [-0.81, 0.59, 0],
        [-0.81, 0.59, 0],
        [0.92, 0, 0.4],
        [0.92, 0, -0.4],
        [0.29, -0.86, 0.4],
        [0.29, -0.86, -0.4],
        [0.29, 0.86, 0.4],
        [0.29, 0.86, -0.4],
        [-0.75, -0.56, 0.4],
        [-0.75, -0.56, -0.4],
        [-0.75, 0.56, -0.4],
        [-0.75, 0.56, 0.4],
        [0.71, 0, 0.7],
        [0.71, 0, -0.7],
        [0.22, 0.6745, 0.7],
        [0.22, 0.6745, -0.7],
        [0.22, -0.6745, 0.7],
        [0.22, -0.6745, -0.7],
        [-0.57, 0.4189, 0.7],
        [-0.57, 0.4189, -0.7],
        [-0.57, -0.4189, 0.7],
        [-0.57, -0.4189, -0.7]
      ];
      console.log(nodeCorr);
      let objects = [];
      this.scene.background = new Color(0x0a1728);
      this.cam = new PerspectiveCamera(
        50,
        window.innerWidth / window.innerHeight,
        0.1,
        1000
      );

      this.cam.position.z = 10;

      this.renderer = new WebGLRenderer({
        canvas: canvas,
        antialias: true,
        alpha: true
      });

      this.renderer.setSize(window.innerWidth, window.innerHeight);

      this.resizeCanvas(window.innerWidth, window.innerHeight);

      this.control = new OrbitControls(this.cam, this.renderer.domElement);

      this.lights = [];
      this.lights[0] = new PointLight(0xffffff, 2, 0);
      this.lights[1] = new PointLight(0xffffff, 2, 0);
      this.lights[2] = new PointLight(0xffffff, 2, 0);

      this.lights[0].position.set(0, 100, 0);
      this.lights[1].position.set(50, 100, 50);
      this.lights[2].position.set(-50, -100, -50);

      for (let i = 0; i < this.lights.length; i++) {
        this.scene.add(this.lights[i]);
      }

      let commoncolor = 0x0a1b2f;
      let highlightcolor = 0x000000;
      this.geometry = new IcosahedronBufferGeometry(1, 2);
      this.meshMaterial = new MeshPhongMaterial({
        color: 0x0a1b2f,
        emissive: 0x072534,
        flatShading: true
      });
      this.lineMaterial = new LineBasicMaterial({
        color: 0x3ee4ee,
        opacity: 1,
        linewidth: 5
      });

      Object.keys(VirusData).forEach(v => {
        this.group = new Group();
        this.group.position.set(
          getRandomInt(-9, 9),
          getRandomInt(-9, 9),
          getRandomInt(-9, 9)
        );
        this.group.name = v;
        this.mesh = new Mesh(this.geometry, this.meshMaterial);
        this.line = new LineSegments(this.geometry, this.lineMaterial);
        this.group.add(this.mesh);
        this.group.add(this.line);

        Object.size = function(obj) {
          var size = 0,
            key;
          for (key in obj) {
            if (obj.hasOwnProperty(key)) size++;
          }
          return size;
        };
        let size = Object.size(VirusData[v]);
        let thisX = this.group.position.x;
        let thisY = this.group.position.y;
        let thisZ = this.group.position.z;
        // this.cylinderGeo = new CylinderGeometry(0.2, 0.2, 1, 16);
        // this.cylinder = new Mesh(this.cylinderGeo, this.meshMaterial);
        // this.cylinder.position.x = nodeCorr[0][0] + thisX;
        // this.cylinder.position.y = nodeCorr[0][1] + thisY;
        // this.cylinder.position.z = nodeCorr[0][2] + thisZ;
        // this.cylinder.lookAt(thisX, thisY, thisZ)
        for(let i = 0; i < size; i++) {
            this.cylinderGeo = new CylinderGeometry(0.2, 0.2, 2, 16);
            this.cylinder = new Mesh(this.cylinderGeo, this.meshMaterial);
            this.cylinder.position.x = nodeCorr[i][0]+ thisX;
            this.cylinder.position.y = nodeCorr[i][1] + thisY;
            this.cylinder.position.z = nodeCorr[i][2] + thisZ;
            this.cylinder.lookAt(thisX, thisY, thisZ)
             this.scene.add(this.cylinder)
        }
        // this.group.add(this.cylinder);
       
        this.scene.add(this.group);
      });

      function getRandomInt(min, max) {
        min = Math.ceil(min);
        max = Math.floor(max);
        return Math.floor(Math.random() * (max - min)) + min;
      }
      this.scene.traverse(function(children) {
        if (children.name.length > 0) {
          objects.push(children);
        }
      });

      this.raycaster = new Raycaster();
      this.mouse = new Vector2();
      this.onMouseClick = event => {
        event.preventDefault();
        this.mouse.x = (event.clientX / window.innerWidth) * 2 - 1;
        this.mouse.y = -(event.clientY / window.innerHeight) * 2 + 1;
        // console.log(this.mouse.x)
        this.raycaster.setFromCamera(this.mouse, this.cam);
        this.intersects = this.raycaster.intersectObjects(objects, true);
        // console.log(this.intersects);
        if (this.intersects.length > 0) {
          this.virus = this.intersects[0].object.parent.name;
          //   console.log(this.virus);
          //   console.log(VirusData[this.virus]);
          $(".tooltip").css("display", "block");
        } else {
          $(".tooltip").css("display", "none");
        }
      };

      this.onMouseMove = event => {
        this.mouse.x = (event.clientX / window.innerWidth) * 2 - 1;
        this.mouse.y = -(event.clientY / window.innerHeight) * 2 + 1;
        this.raycaster.setFromCamera(this.mouse, this.cam);
        this.intersects = this.raycaster.intersectObjects(objects, true);
        if (this.intersects.length > 0) {
          $("html,body").css("cursor", "pointer");
          //   this.intersects[0].object.parent.children[0].material.color.set(highlightcolor)
        } else {
          $("html,body").css("cursor", "default");
        }
      };

      //   this.onMouseOver = event => {
      //     this.mouse.x = (event.clientX / window.innerWidth) * 2 - 1;
      //     this.mouse.y = -(event.clientY / window.innerHeight) * 2 + 1;
      //     this.raycaster.setFromCamera(this.mouse, this.cam);
      //     this.intersects = this.raycaster.intersectObjects(objects, true);
      //     if (this.intersects.length > 0) {
      //         console.log(this.intersects[0].object.parent.children[0])
      //     //   this.intersects[0].object.parent.children[0].material.color = new Color(highlightcolor)
      //     }
      //   };

      window.addEventListener("mousedown", this.onMouseClick, false);
      window.addEventListener("mousemove", this.onMouseMove, false);
      //   window.addEventListener("mouseover", this.onMouseOver, false);
    },

    loop() {
      this.renderer.render(this.scene, this.cam);
      requestAnimationFrame(this.loop);
    },

    resizeCanvas() {
      let width = window.innerWidth;
      let height = window.innerHeight;
      this.renderer.setSize(width, height);
      this.cam.aspect = width / height;
      this.cam.updateProjectionMatrix();
    }
  }
};
</script>

<style scoped>
body {
  margin: 0;
}
canvas {
  /* background: #0a1728; */
  width: 100%;
  height: 100%;
}

.tooltip {
  display: none;
  width: 50%;
  height: 50%;
  position: absolute;
  top: 20%;
  left: 45%;
  background: rgb(35, 51, 72);
}

.virus-name {
  color: white;
  padding-left: 3rem;
  padding-top: 1rem;
  font-size: 24px;
  letter-spacing: 1px;
  font-style: italic;
}
</style>