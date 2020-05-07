<template>
  <div class="clustercontainer" ref="container">
    <svg :viewBox="[0, 0, width, height]" :width="width" :height="height" ref="cluster">
      <g
        v-for="(family,i) in allFamily"
        :id="family.familyName"
        :key="i"
        :transform="`translate(${i%4*250+50+margin.left}, ${margin.top + Math.floor(i/4) * 250})`"
      >
        <text
          :transform="`translate(100, 220)`"
          fill="white"
          text-anchor="middle"
        >{{family.familyName}}</text>
        <g
          v-for="(genus, g) in family.allGenus"
          :id="genus.genusName"
          :key="g"
          :transform="`translate(${g % 4 * 40 + 20}, ${Math.floor(g/4) * 40 + 60})`"
        >
          <circle
            :r="10"
            :fill="dotColor(genus.direct)"
            @mouseover="dotHovered = true, hoveredAnimal =genus.genusName, toolTipX=i%4*250+50+margin.left + g % 4 * 40 + 20, toolTipY = margin.top + Math.floor(i/4) * 250 + Math.floor(g/4) * 40 + 60"
            @mouseleave="dotHovered = false, hoveredAnimal=null, toolTipX=null, toolTipY=null"
            @click="selectAnimal(genus.genusName)"
          />
        </g>
      </g>
      <g v-if="dotHovered" :transform="getToolTipPosition(toolTipX, toolTipY)">
        <rect fill="#000000" :width="300" :height="300" stroke="#FF6496" />

        <text
          class="commonName"
          fill="#FF6496"
          :transform="`translate(14, 30)`"
        >{{matchName(hoveredAnimal)}}</text>
        <line stroke="#FF6496" x1="0" y1="44" x2="300" y2="44" />
        <text class="genusName" fill="#929292" :transform="`translate(14, 64)`">{{hoveredAnimal}}</text>
        <text
          fill="#ffffff"
          :transform="`translate(14, 88)`"
        >Also reservoir of {{findOtherVirus(selectedVirus, hoveredAnimal).length}} other virus</text>
        <g
          v-for="(virus,v) in findOtherVirus(selectedVirus, hoveredAnimal)"
          :key="v"
          :transform="`translate(20, ${120 + 24 * v})`"
        >
          <circle :r="4" :fill="dotColor(virus.direct)" :transform="`translate(0, -4)`" />
          <text
            class="foundVirus"
            fill="#ffffff"
            :transform="`translate(14, 0)`"
          >{{virus.virusName}}</text>
        </g>

        <!-- <text fill="#ffffff" :transform="`translate(14, 100)`">{{findOtherVirus(selectedVirus, hoveredAnimal)}}</text> -->
      </g>
    </svg>
  </div>
</template>

<script>
import * as d3 from "d3";
import VirusData from "~/assets/virus-grouped.json";
import AnimalName from "~/assets/animalName.json";
export default {
  props: ["allFamily", "selectedVirus"],
  name: "Cluster",
  data() {
    return {
      // selectedVirus: this.selectedVirusName,
      VirusData: VirusData,
      margin: {
        top: 50,
        left: 0,
        right: 0,
        bottom: 50
      },
      height: 0,
      width: 0,
      dotHovered: false,
      hoveredAnimal: "",
      hoveredAnimalCommonName: "",
      toolTipX: null,
      toolTipY: null,
      selectedAnimal: null
      // foundVirus: null
    };
  },
  mounted() {
    this.setup();
  },
  methods: {
    setup() {
      this.width = this.$refs.container.offsetWidth;
      this.height = 1000;
    },
    dotColor(indicator) {
      if (indicator == true) {
        return "#FF6496";
      } else {
        return "#FDD880";
      }
    },
    forceLayout(nodes) {
      let force = d3
        .forceSimulation(nodes)
        .force("center", d3.forceCenter(200, 200));
    },
    matchName(genus) {
      let nameLibray = AnimalName;
      let matched = nameLibray.find(({ genusName }) => genusName === genus);
      return matched.commonName;
    },
    findOtherVirus(currentVirus, genus) {
      let foundVirus = [];
      let allVirus = VirusData;
      Object.keys(allVirus).forEach(e => {
        if (e != currentVirus) {
          let otherVirus = allVirus[e];
          // console.log(otherVirus)
          for (let family in otherVirus) {
            let allG = otherVirus[family].genus;
            for (let i = 0; i < allG.length; i++) {
              if (allG[i].genusName === genus) {
                let obj = {};
                obj.virusName = e;
                obj.direct = allG[i].direct;
                foundVirus.push(obj);
              }
            }
          }
        }
      });
      return foundVirus;
    },

    getToolTipPosition(x, y) {
      if (x + 300 > this.$refs.container.offsetWidth) {
        return `translate(${this.$refs.container.offsetWidth - 320}, ${y + 8})`;
      }
      return `translate(${x + 8}, ${y + 8})`;
    },

    selectAnimal(genusName) {
      this.selectedAnimal = genusName;
      this.$emit("selectAnimal", genusName);
    }
  },
  computed: {}
};
</script>

<style scoped>
p {
  color: white;
  margin-left: 40px;
  margin-right: 60px;
  margin-top: 30px;
  font-size: 20px;
}

.container {
  height: 100vh;
}

.commonName {
  font-size: 20px;
  font-weight: bold;
}

.genusName {
  font-style: oblique;
}

.foundVirus {
  font-size: 14px;
}

.clustercontainer {
  padding-left: 200px;
}
</style>
