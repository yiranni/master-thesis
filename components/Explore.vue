<template>
  <div class="main">
    <div class="selectVirus">
      <h1>
        <span>
          <span class="select-wrapper">
            <select id="virus-select" v-model="selected" @change="changeVirus">
              <option
                v-for="option in VirusOptions"
                :key="option.value"
                v-bind:value="{value: option.value, familyCount: option.familyCount, genusCount: option.genusCount}"
                :virusName="option.value"
              >{{ option.value }}</option>
            </select>
            <select id="width_tmp_select">
              <option id="width_tmp_option"></option>
            </select>
          </span>
        </span> has
        <span class="animal-count">{{selected.genusCount}}</span> potential wildlife reservoirs in
        <span class="family-count">{{selected.familyCount}}</span> families
      </h1>

      <Cluster v-bind:allFamily="allFamily" :selectedVirus="selectedVirusName" v-on:selectAnimal="updateSelectedAnimal($event)"/>
      <!-- <Title v-bind:allFamily="allFamily" /> -->
    </div>
    <div class="detailSection">
      <div v-if="!animalSelected">
        <InfoTitle v-bind:selectedVirusName="selectedVirusName" />
        <InfoDescript v-bind:selectedVirusDescrip="selectedVirusDescrip" />
      </div>
      <div v-if="animalSelected">
        <AnimalDetails v-bind:selectedAnimal="selectedAnimal"/>
      </div>
    </div>
  </div>
</template>

<script>
import VirusInfo from "~/assets/data/virusInfo.json";
import InfoTitle from "~/components/InfoTitle.vue";
import InfoDescript from "~/components/InfoDescript.vue";
import Cluster from "~/components/Cluster.vue";
import VirusGroup from "~/assets/virus-grouped.json";
import Title from "~/components/Title.vue";
import AnimalDetails from "~/components/AnimalDetails.vue"
export default {
  name: "Explore",
  components: {
    InfoTitle,
    InfoDescript,
    Cluster,
    Title,
    AnimalDetails
  },
  props: {
    VirusOptions: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      VirusInfo: VirusInfo,
      selectedVirusName: null,
      selectedVirusDescrip: null,
      selected: {
        value: "SARS",
        familyCount: 7,
        genusCount: 28
      },
      VirusGroup: VirusGroup,
      allFamily: null,
      animalSelected: false,
      selectedAnimal: null,
      selectedVirusReservoir: null,
      direct: null
    };
  },
  mounted() {
    document.getElementById("virus-select").addEventListener(
      "change",
      function() {
        var resize = document.getElementById("virus-select");
        var hidden_opt = document.getElementById("width_tmp_option");
        hidden_opt.innerHTML = resize.options[resize.selectedIndex].textContent;
        var hidden_sel = document.getElementById("width_tmp_select");
        hidden_sel.style.display = "initial";
        resize.style.width = hidden_sel.clientWidth * 2.2 + "px";
        hidden_sel.style.display = "none";
      },
      false
    );
    this.changeVirus();
    this.findRelation();
  },

  methods: {
    changeVirus(event) {
      this.selectedVirusName = this.selected.value;
      this.selectedVirusDescrip = this.virusData[this.selected.value];
      this.$emit("changeVirusName", this.selectedVirusName);

      let selectedVirusReservoir = this.VirusGroup[this.selected.value];
      this.allFamily = [];
      Object.keys(selectedVirusReservoir).forEach(f => {
        let selectedVirusReservoirFamily = {};
        selectedVirusReservoirFamily.familyName = f;
        let genusInFamily = selectedVirusReservoir[f].genus;
        selectedVirusReservoirFamily.allGenus = genusInFamily;
        this.allFamily.push(selectedVirusReservoirFamily);
      });
      this.animalSelected = false
      // console.log(this.allFamily)
    },

    updateSelectedAnimal(updatedAnimal) {
      this.selectedAnimal = updatedAnimal;
      this.animalSelected = true;
    },

    findRelation() {
      this.direct = this.VirusGroup[this.selectedVirusName]
    }

  },
  computed: {
    virusData() {
      return VirusInfo;
    }
  },
  created() {}
};
</script>

<style scoped>
.main {
  width: 100vw;
  height: 100vh;
  background-color: #313437;
  z-index: 0;
}

.selectVirus {
  /* margin-left: 200px; */
  color: white;
  padding-right: 560px;
}

h1 {
  padding-top: 80px;
  padding-left: 200px;
  font-size: 24px;
  font-weight: normal;
  line-height: 36px;
}

h1 .virus-name {
  font-weight: bold;
}

h1 .animal-count {
  font-weight: bold;
  color: #ff6496;
}

h1 .family-count {
  font-weight: bold;
  color: #ff6496;
}

#virus-select {
  -webkit-appearance: none;
  -moz-appearance: none;
  -ms-appearance: none;
  -o-appearance: none;
  appearance: none;
  background: none;
  border-radius: 0;
  border-width: 0 0 2px 0;
  border-color: #ff6496;
  font-size: 24px;
  font-weight: bold;
  color: white;
  padding-bottom: 6px;
  cursor: pointer;
  width: 120px;
  text-align-last: center;
  background: url("../assets/arrow.svg") no-repeat right;
}

.select-wrapper {
  overflow: auto;
  position: relative;
  font-size: 14px;
  height: 40px;
}

#virus-select:focus {
  outline: 0;
}

#virus-select option {
  font-size: 12px;
}
#width_tmp_select {
  display: none;
}

.detailSection {
  height: 100%;
  width: 540px;
  position: fixed;
  z-index: 1;
  top: 0;
  right: 0;
  background-color: #000000;
  overflow-x: hidden;
  /* padding-top: 20px; */
  padding-top: 80px;
}

p {
  color: white;
}
</style>
