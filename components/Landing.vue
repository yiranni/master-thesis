<template>
  <div class="main">
    <div class="selectVirus">
      <h1 style="font-size: 52px; text-shadow: 3px 3px #b60b0b">Global Journey of Zoonotic Diseases</h1>
      <p
        style="padding-top: 2em; font-style:oblique; color: #A6A3A3"
      >An analysis of how global wildlife trading is related to the zoonotic diseases.</p>
    </div>
    <div class="detail">
      <div class="subheader">
        <h2>COVID-19 Outbreak</h2>
      </div>
      <div class="content">
        <p>
          COVID-19 has caused a worldwide health pandemic. The origin of the outbreak is believed to have been in a wildlife market in
          <b
            style="color: #b60b0b;"
          >Wuhan</b>, China, where sold live and dead wildlife and domestic animals for human consumption. The exact origins of COVID-19 are still unknown, but it is suspected to have originated in bats and may have jumped to humans via an intermediary wildlife animal species.
        </p>
      </div>
      <div style="text-align: center; padding-top: 2rem">
        <img src="../assets/ChinaMap.png" style="width: 30vw" />
      </div>
    </div>
    <div class="detail">
      <div class="subheader">
        <h2>Reservoirs of Zoonotic Diseases</h2>
      </div>
      <div class="content">
        <p>
          The outbreak of COVID-19 brought the term
          <b style="color: #b60b0b;">"zoonotic disease"</b> to the public. Zoonotic diseases. also known as zoonoses, are illnesses caused by germs that are passed between animals and human with non-human animal origin.
        </p>
        <p>Zoonotic diseases are much more common than people may realize: within hundreds of emerging infectious diseases (EIDs) reported since 1940, 60.3% of EID events are caused by zoonotic pathogens.</p>
        <div
          style="align-items:center; justify-content: center; display: flex; padding-top: 4em; padding-bottom:4em"
        >
          <img src="../assets/summary.svg" style="width: 40vw" />
        </div>
        <p
          style="font-size: 32px; font-weight: bold; color: #b60b0b; font-family: DM Serif Display, serif"
        >Reservoirs of 17 zoonotic diseases</p>
        <p
          style="padding-top: 2em; font-style:oblique; color: #A6A3A3; font-size: 14px; line-height: 16px"
        >* direct: this genus is the reservoir of selected zoonotic disease; indirect: this genus is not but other genera in the same taxonomic family is the reservoir of selected zoonotic disease.</p>
      </div>
    </div>
    <div style="align-items:center; justify-content: center; display: flex;">
      <AllReservoirs />
    </div>

    <div class="subheader">
      <h2 style>Tradings Included Zoonotic Disease Reservoirs</h2>
    </div>
    <div class="detail" style="padding-top: 0">
      <div class="content">
        <p
          style="font-size: 32px; font-weight: bold; color: #b60b0b; font-family: DM Serif Display, serif"
        >Overview of Risk on Trading Mammals</p>
        <p>Among all mammals traded as live during 2018 - 2019, almost every animal is a potential reservoir of any zoonotic diseases stated above.</p>
      </div>
    </div>
    <div style="align-items:center; justify-content: center; display: flex;">
      <TradeWithVirus />
    </div>

    <div class="detail" style="padding-top: 0">
      <div class="content">
        <p
          style="font-size: 32px; font-weight: bold; color: #b60b0b; font-family: DM Serif Display, serif"
        >Breakdown into 17 Zoonotic Diseases</p>
      </div>
    </div>
    <div style="align-items:center; justify-content: center; display: flex;">
      <TradingDetails />
    </div>

    <div class="detail" style="padding-top: 0">
      <div class="content">
        <p
          style="font-size: 32px; font-weight: bold; color: #b60b0b; font-family: DM Serif Display, serif"
        >Risk Assessment for Mammals</p>

        <p>Based on 17 zoonotic diseases and their potential reservoirs provided above, I assessed the risky score of each genus by:</p>
        <div>
          <p>
            <b style="color: #b60b0b;">+0</b> for not as reservoir of X disease
          </p>
          <p>
            <b style="color: #b60b0b;">+1</b> for as indirect reservoir of X disease
          </p>
          <p>
            <b style="color: #b60b0b;">+2</b> for as direct reservoir of X disease
          </p>
        </div>
        <p>The top 6 risky wildlife mammals are: Chimpanzee, Orangutan, Canine, Cat, Fox, and Monkey.</p>
      </div>
    </div>
    <div style="align-items:center; justify-content: center; display: flex;">
      <RiskAssessment />
    </div>
    <div class="detail">
      <div class="subheader">
        <h2>Demand for Wildlife Animals</h2>
      </div>

      <div class="content" style="padding-bottom: 4em">
        <p>Suistainable, legal, and equitable wildlife trade could enhance rural livelihoods and conserve biological diversity. However, a decent amount of tradings are promoted by personal demand and under illegal trading. Illegal wildlife trading is even more dangerous on spreading zoonotic diseases due to the lack of disease testing.</p>
        <p class="quote" style="padding-bottom: 4em; padding-top: 4em">
          <span style="color: #b60b0b; font-size: 24px; font-weight: bold;">"</span> People in many countries are accustomed to a lifestyle which fuels demand for wildlife.
          They expect access to a variety of seafoods, leather goods, timbers, medicinal ingredients and textiles. At the other end,
          extreme poverty means some people see wildlife as valuable barter for trade.
          <span
            style="color: #b60b0b;font-size: 24px; font-weight: bold"
          >"</span>
        </p>
        <p
          style="font-size: 32px; font-weight: bold; color: #b60b0b; font-family: DM Serif Display, serif"
        >Unfounded Medical Benefits</p>
        <p>
          Wildlife such as rhinos and pangolins are poached and traded to countries where people believe they have no proven medicinal value. Pangolin scales and rhino horns are made of keratin,
          which is the material that makes up fingernails and hair. However, they are used in traditional Chinese medicine to help illness range from lactation difficulties to arthritis.
        </p>
        <div style="align-items:center; justify-content: center; display: flex;">
          <img
            src="../assets/rhino.svg"
            style="width: 20vw; flex-grow: 1; padding-left: 4em; padding-right: 4em"
          />
          <img
            src="../assets/pangolin.svg"
            style="width: 20vw; height: 6vw; flex-grow: 1; padding-left: 4em; padding-right: 8em; padding-top: 2em"
          />
        </div>
        <p
          style="font-size: 32px; font-weight: bold; color: #b60b0b; font-family: DM Serif Display, serif"
        >Symbol of Wealth</p>
        <p>
          Endangered wildlife are more vulnerable to be involved in trading. The rarity of wildlife increases its economic value. Animals such as elephants and tigers are hunted and traded in forms of ivory and skin.
          After being processed, elephant ivories and tiger skins are consumed as luxury decorations and furs as symbols of wealth.
        </p>

        <div style="align-items:center; justify-content: center; display: flex;">
          <img
            src="../assets/elephant.svg"
            style="width: 20vw; flex-grow: 1; padding-left: 4em; padding-right: 4em"
          />
          <img
            src="../assets/tiger.svg"
            style="width: 12vw; flex-grow: 1; padding-left: 4em; padding-right: 8em; padding-top: 2em"
          />
        </div>
        <div style="text-align: center; padding-top: 4em; padding-bottom: 4em; display: flex">
         
        </div>
        <p
          style="font-size: 32px; font-weight: bold; color: #b60b0b; font-family: DM Serif Display, serif"
        >Law Enforcements and Illegal Trading</p>
        <p>
          CITES is a treaty initiated in 1973 to protect endangered plants and animals.
          By now,
          <b
            style="color: #b60b0b;"
          >183</b> state/ regional economic intergration organizations have entered into the force.
          Under terms of CITES, Parties are required to submit annual reports documenting imports and exports to the CITES.
        </p>
      </div>
      <div style="align-items:center; justify-content: center; display: flex;">
        <CITESParty />
      </div>

      <div class="content" style="padding-bottom: 6em">
        <p>
          However, illegals trading is still untrackable. In 2019,
          <b style="color: #b60b0b;">17</b> African elephant tradings were reported to CITES, but it is estimated that
          <b
            style="color: #b60b0b;"
          >20,0000</b> African elephants are killed each year.
        </p>

        <div>
           <div style="align-items:center; justify-content: center; display: flex;">
            <div>
              <img
                src="../assets/elephant.svg"
                style="padding-right: 5vw; padding-left: 5vw; width: 20vw"
              />
            </div>
            <div style="padding-top: 4em; padding-bottom: 4em">
              <p style="font-size: 42px; font-weight: bold; color: #b60b0b;">X 17</p>
              <p
                style="font-size: 32px; font-weight: bold; color: #b60b0b; font-style: oblique"
              >Reported to CITES</p>
            </div>
          </div>
          <div style="display: flex; margin-bottom: 4em; margin-top: 4em">
            <div class="col">
              <img
                src="../assets/elephant.svg"
                style="padding-right: 5vw; padding-left: 5vw;width: 20vw"
              />
            </div>
            <div style="padding-top: 4em; padding-bottom: 4em; ">
              <p style="font-size: 42px; font-weight: bold; color: #b60b0b;">X 20,000</p>
              <p
                style="font-size: 32px; font-weight: bold; color: #b60b0b; font-style: oblique"
              >Estimately Killed</p>
            </div>
          </div>
        </div>
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
// import SummaryExportTerm from "~/components/SummaryExportTerm.vue";
import SummaryExportCountry from "~/components/SummaryExportCountry.vue";
import SummaryImportCountry from "~/components/SummaryImportCountry.vue";
import AnimalTradeQuantity from "~/components/AnimalTradeQuantity.vue";
import TradeWithVirus from "~/components/TradeWithVirus.vue";
import AllReservoirs from "~/components/AllReservoirs.vue";
import CITESParty from "~/components/CITESParty.vue";
import TradingDetails from "~/components/TradingDetails.vue";
import Elephant from "~/components/Elephant.vue";
import RiskAssessment from "~/components/RiskAssessment.vue";
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
    // SummaryExportTerm,
    SummaryExportCountry,
    SummaryImportCountry,
    AnimalTradeQuantity,
    TradeWithVirus,
    AllReservoirs,
    CITESParty,
    TradingDetails,
    Elephant,
    RiskAssessment
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
    // document.getElementById("virus-select").addEventListener(
    //   "change",
    //   function() {
    //     var resize = document.getElementById("virus-select");
    //     var hidden_opt = document.getElementById("width_tmp_option");
    //     hidden_opt.innerHTML = resize.options[resize.selectedIndex].textContent;
    //     var hidden_sel = document.getElementById("width_tmp_select");
    //     hidden_sel.style.display = "initial";
    //     resize.style.width = hidden_sel.clientWidth * 3.5 + "px";
    //     hidden_sel.style.display = "none";
    //   },
    //   false
    // );
    // document.getElementById("topButton").addEventListener("click", function() {
    //   document.body.scrollTop = 0;
    //   document.documentElement.scrollTop = 0;
    // });
    // document.addEventListener("scroll", this.handleScroll);
    // this.changeVirus();
    // this.handleScroll();
  },

  methods: {
    // changeVirus(event) {
    //   this.selectedVirusName = this.selected.value;
    //   this.selectedVirusDescrip = this.virusData[this.selected.value];
    //   this.$emit("changeVirusName", this.selectedVirusName);
    //   let selectedVirusReservoir = this.VirusGroup[this.selected.value];
    //   this.allFamily = [];
    //   Object.keys(selectedVirusReservoir).forEach(f => {
    //     let selectedVirusReservoirFamily = {};
    //     selectedVirusReservoirFamily.familyName = f;
    //     let genusInFamily = selectedVirusReservoir[f].genus;
    //     selectedVirusReservoirFamily.allGenus = genusInFamily;
    //     this.allFamily.push(selectedVirusReservoirFamily);
    //   });
    //   this.animalSelected = false;
    //   // console.log(this.allFamily)
    // },
    // handleScroll() {
    //   this.scrolled = window.scrollY > 0;
    // }
  },
  computed: {
    // virusData() {
    //   return VirusInfo;
    // }
  },
  created() {}
};
</script>

<style scoped>
.main {
  background-color: #131010;
  z-index: 0;
  padding-left: 12em;
  padding-right: 12em;
  padding-top: 4em;
  font-family: Roboto, sans-serif;
}

.selectVirus {
  /* margin-left: 250px; */
  color: white;
  /* padding-right: 560px; */
  padding-top: 80px;
  text-align: center;
}

h1 {
  /* padding-top: 80px; */

  line-height: 54px;
}

h1 b {
  font-size: 48px;
  color: #b60b0b;
}

h1 .virus-name {
  font-weight: bold;
}

h1 .animal-count {
  font-weight: bold;
  color: #b60b0b;
}

h1 .family-count {
  font-weight: bold;
  color: #b60b0b;
}
.sub-header {
  font-size: 32px;
  margin-top: 30px;
  font-weight: normal;
  color: #b60b0b;
}

.highlighted {
  color: #b60b0b;
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
  border-color: #b60b0b;
  font-size: 42px;
  font-weight: bold;
  color: #b60b0b;
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
  margin-top: 4em;
  padding-left: 6em;
  padding-right: 6em;
  /* width: 800px; */
}

p {
  color: white;
  font-size: 20px;
  font-weight: 300;
  line-height: 30px;
  padding-bottom: 20px;
}
.subheader {
  color: white;
  text-align: center;
  text-shadow: 3px 3px #b60b0b;
  font-size: 28px;
  position: sticky;
  top: 0;
  background: #131010;
  padding-top: 3em;
  padding-bottom: 2em;
  z-index: 20;
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
  color: #b60b0b;
}

#topButton {
  font-size: 20px;
  color: #b60b0b;
  background: none;
  padding-bottom: 6px;
  border: none;
  border-bottom: 2px solid #b60b0b;
}

#topButton:focus {
  outline: 0;
  cursor: pointer;
}

.detail {
  padding-left: 6em;
  padding-right: 6em;
  padding-top: 4em;
}

.quote {
  font-size: 22px;
  font-style: oblique;
  padding-left: 6em;
  padding-right: 6em;
}

h2 {
  background-color: #131010;
  font-family: "DM Serif Display", serif;
}

h1 {
  font-family: "DM Serif Display", serif;
}
</style>