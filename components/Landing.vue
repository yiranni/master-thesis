<template>
  <div class="main">
    <div class="selectVirus">
      <h1>
        <b>Global Journey</b> of
      </h1>
      <h1>
        <span>
          <span class="select-wrapper">
            <select id="virus-select" v-model="selected" @change="changeVirus">
              <option
                v-for="option in VirusOptions"
                :key="option.value"
                v-bind:value="{value: option.value}"
                :virusName="option.value"
              >{{ option.value }}</option>
            </select>
            <select id="width_tmp_select">
              <option id="width_tmp_option"></option>
            </select>
          </span>
        </span>
      </h1>
      <div class="content">
        <p>
          Many people come into contact with animals in their daily lives,
          at home and away from home, in urban and rural areas.
          Animals bring benefits to humans as food, livelihoods, and companionship.
          However, animals could also carry harmful germs that spread to humans and cause illness,
          even death, which are known as
          <a
            href="#"
          >zoonotic diseases</a>.
        </p>

        <p>Zoonotic diseases are much more common than most people realize: within hundreds of emerging infectious diseases (EIDs) reported since 1940, 60.3% of EID events are caused by zoonotic pathogens, which have a non-human animal source.</p>

        <p>
          Zoonotic diseases are huge concern because they are usually not able to exterminate a disease if the reservoir host is a wildlife species.
          For instance, plague, also known as the Black Death during the Middle Ages, killed millions of people during World Wars I and II. Even though it has been eradicated in most parts of the world with the help of modern medicine, it still occurs in the United States. This is not due to any public health related shortcomings but because this disease has spread to North American wildlife.
        </p>
        <p>
          Wildlife animal trading is a multi-billion-dollar industry. In 2019, a total amount of
          <b>61225</b> wildlife animals were traded globally in various terms, including live, skin, leather, specimens, etc. Let's start looking at the top 3 trading terms.
        </p>
      </div>

      <h2 class="sub-header">2019 Global Wildlife Animal Trading Amount by Terms</h2>
      <SummaryExportTerm />
      <div class="content">
        <p>
          You may ask how the vast trading becomes the culprit of zoonotic diseases. Let's take a look at the trading volum of wildlife animals that are reservoirs of
          <b
            class="selectedVirus"
          >{{selected.value}}</b>
          . (You can select a different virus other than {{selected.value}} at the top of the page.)
        </p>
      </div>
      <!-- <Title v-bind:allFamily="allFamily" /> -->
    </div>
    <div class="figure-container">
      <div class="figure">
        <Shadow width="400" transform="translate(100,660)" />
        <Money width="160" transform="translate(-260,640)" />
        <Virus width="400" transform="translate(110,200) " />
        <Tail width="160" transform="translate(-50,200)" />
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
import AnimalDetails from "~/components/AnimalDetails.vue";
import SummaryExportTerm from "~/components/SummaryExportTerm.vue";

// svg imgs
import Virus from "~/components/Virus.vue";
import Tail from "~/components/Tail.vue";
import Shadow from "~/components/Shadow.vue";
import Money from "~/components/Money.vue";
export default {
  name: "Landing",
  components: {
    Virus,
    Tail,
    Shadow,
    Money,
    SummaryExportTerm
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
        value: "SARS"
      },
      VirusGroup: VirusGroup,
      scrolled: false
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
        resize.style.width = hidden_sel.clientWidth * 3.5 + "px";
        hidden_sel.style.display = "none";
      },
      false
    );

    // document.addEventListener("scroll", this.handleScroll);
    this.changeVirus();
    this.handleScroll()
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
      this.animalSelected = false;
      // console.log(this.allFamily)
    },
    handleScroll() {
      this.scrolled = window.scrollY > 0;
    }
  },
  computed: {
    virusData() {
      return VirusInfo;
    }
  },
  created() {
   
  }
};
</script>

<style scoped>
.main {
  width: 100vw;
  /* height: 100; */
  background-color: #313437;
  z-index: 0;
}

.selectVirus {
  margin-left: 250px;
  color: white;
  padding-right: 560px;
  padding-top: 80px;
}

h1 {
  /* padding-top: 80px; */

  line-height: 54px;
}

h1 b {
  font-size: 48px;
  color: #f73179;
}

h1 .virus-name {
  font-weight: bold;
}

h1 .animal-count {
  font-weight: bold;
  color: #f73179;
}

h1 .family-count {
  font-weight: bold;
  color: #f73179;
}
.sub-header {
  font-size: 32px;
  margin-top: 30px;
  font-weight: normal;
  color: #f73179;
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
  border-color: #f73179;
  font-size: 42px;
  font-weight: bold;
  color: #f73179;
  padding-bottom: 6px;
  cursor: pointer;
  width: 200px;
  text-align-last: left;
  background: url("../assets/arrow.svg") no-repeat right;
}

.select-wrapper {
  overflow: auto;
  position: relative;
  font-size: 32px;
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

.content {
  margin-top: 50px;
  width: 800px;
}

p {
  color: white;
  font-size: 20px;
  font-weight: 300;
  line-height: 30px;
  padding-bottom: 20px;
}

.figure-container {
  position: fixed;
  height: 100%;
  width: 600px;
  top: 0;
  right: 0;
  overflow-x: hidden;
  /* background-color: black; */
}

.figure {
  /* padding-top: 200px; */
  /* padding-right: 30px; */
  height: 100%;
}

a {
  color: white;
  font-weight: bold;
  text-decoration: none;
  padding-bottom: 6px;
  border-bottom: 2px solid white;
  /* line
  -height: 48px; */
}

.selectedVirus {
  color: #f73179;
}
</style>