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
          A huge number of these animal species have been under international trading.
        </p>
        <p>Endangered animals are more vulnerable to be the target of wildlife trading because of their rarity which increased their economic value.</p>
        <p>
          <a href="#">CITES</a> prvides a legal framework for regulating international trade in sprecies threatened or potentially threatened by that trade. Under terms of CITES, Parties are required to submit annual reports to the CITES. In 2019,
          <b>64148</b> species were reported by exporters, and
          <b>61224</b> species were reported by importers.
        </p>
      </div>

      <h2 class="sub-header">2019 Top 5 Import Countries</h2>
      <SummaryImportCountry />
      <h2 class="sub-header">2019 Top 5 Export Countries</h2>
      <SummaryExportCountry />
      <div class="content">
        <p>Wildlife animal species are being traded in various forms, and the top 3 forms of trading in 2019 are skin, live, and leather.</p>
      </div>
      <h2 class="sub-header">2019 Global Wildlife Animal Trading Amount by Terms</h2>
      <SummaryExportTerm />
      <div class="content">
        <p>
          In 2019, trading data reported to CITES included
          <b>251</b> threatened or potentially threatened wildlife animal species. Among these species, the following have the largest amount
        </p>
      </div>
      <AnimalTradeQuantity />
      <div class="content">
        <p>
          Wildlife animal trading is a multi-billion-dollar industry. Caimans, the species involed in the most numerous international trading, are most common traded in the form of skins. The latest
          <a
            href="#"
          >price</a> of a Caiman skin is USD $270 for selling. 52918 Caimans traded in 2019 involved a cash flow of
        </p>
        <div class="highlighted">
          <h3>52,918 X 270 = USD $14,287,860</h3>
        </div>
      </div>

      <div class="content">
        <p>
          While wildlife animals trading brings profit to both parties, it could also carry harmful germs that spread to humans and cause illness,
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
          Let's take a look at the trading volum of wildlife animals that are reservoirs of
          <b
            class="selectedVirus"
          >{{selected.value}}</b>
          . (You can select a different virus other than {{selected.value}} at the
          <button id="topButton">top</button> of the page.)
        </p>
      </div>
      <TradeWithVirus :selectedVirus="selectedVirusName" />
      <div class="content" style="padding-bottom: 200px">
        <p>If you would like to know more infomation about zoonotic diseases and their reservoirs involved in international tradings. Let's go to Explore Dashboard!</p>
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
import SummaryExportCountry from "~/components/SummaryExportCountry.vue";
import SummaryImportCountry from "~/components/SummaryImportCountry.vue";
import AnimalTradeQuantity from "~/components/AnimalTradeQuantity.vue";
import TradeWithVirus from "~/components/TradeWithVirus.vue";
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
    SummaryExportTerm,
    SummaryExportCountry,
    SummaryImportCountry,
    AnimalTradeQuantity,
    TradeWithVirus
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

    document.getElementById("topButton").addEventListener("click", function() {
      document.body.scrollTop = 0;
      document.documentElement.scrollTop = 0;
    });

    // document.addEventListener("scroll", this.handleScroll);
    this.changeVirus();
    this.handleScroll();
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
  created() {}
};
</script>

<style scoped>
.main {
  /* width: 100vw; */
  /* height: 100; */
  background-color: #313437;
  z-index: 0;
  /* padding-bottom: 200px; */
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

.highlighted {
  color: #f73179;
  font-weight: normal;
  font-style: oblique;
  font-size: 42px;
  padding-top: 20px;
  padding-bottom: 20px;
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

#topButton {
  font-size: 20px;
  color: #f73179;
  background: none;
  padding-bottom: 6px;
  border: none;
  border-bottom: 2px solid #f73179;
}

#topButton:focus {
  outline: 0;
  cursor: pointer;
}
</style>