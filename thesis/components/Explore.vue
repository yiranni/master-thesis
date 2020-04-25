<template>
  <div class="main">
    <div class="section">
      <div class="title">
        <h1>
          <span>
            <span class="select-wrapper">
              <select id="virus-select" v-model="selected">
                <option
                  v-for="option in options"
                  :key="option.value"
                  :value="{value: option.value, familyCount: option.familyCount, genusCount: option.genusCount}"
                  v-on:click="updateVirus"
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
      </div>
    </div>
  </div>
</template>

<script>
import VirusData from "~/assets/virus-grouped.json";
import { library } from "@fortawesome/fontawesome-svg-core";
import { faUserSecret } from "@fortawesome/free-solid-svg-icons";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
export default {
  name: "Explore",
  props: ['option.value'],
  data() {
    return {
      VirusData: VirusData,
      selected: "",
      options: []
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
    this.virusSelection();
    this.updateVirus();
  },

  computed: {},
  methods: {
    virusSelection() {
      const virusData = VirusData;
      Object.keys(virusData).forEach(v => {
        let obj = {};
        obj.text = v;
        obj.value = v;
        obj.virusName = v;
        obj.genusCount = 0;
        Object.keys(virusData[v]).forEach(f => {
          // console.log(f)
          Object.size = function(obj) {
            var size = 0,
              key;
            for (key in obj) {
              if (obj.hasOwnProperty(key)) size++;
            }
            return size;
          };
          let familyCount = Object.size(VirusData[v]);
          obj.familyCount = familyCount;
          // Object.keys(virusData[v][f]).forEach(g => {
          //   console.log(typeof g)
          // })
          // console.log(f+': ' + virusData[v][f].genus.length)
          let allGenus = virusData[v][f].genus;
          obj.genusCount += allGenus.length;
        });

        // console.log(obj);
        this.options.push(obj);
      });
    },
    updateVirus() {
      this.$emit('updateVirus', "virus changed")
      // console.log(this.value)
    }
  }
};
</script>

<style scoped>
.main {
  width: 100vw;
  height: 100vh;
  background-color: #313437;
  z-index: 0;
}

.section {
  margin-left: 200px;
  color: white;
  padding-right: 560px;
}

h1 {
  padding-top: 80px;
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
/* #virus-select::after {
  font-family: FontAwesome;
  content: "\f107";
  font-size: 14px;
  position: absolute;
  top: 12px;
  right: 20px;
  color: #434b67;
  pointer-events: none;
} */

#virus-select:focus {
  outline: 0;
}

#virus-select option {
  font-size: 12px;
}
#width_tmp_select {
  display: none;
}
</style>
