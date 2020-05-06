<template>
  <div class="main">
      <p>{{selectedVirus}}</p>

  </div>
</template>

<script>
import allTrade2019 from "~/assets/data/allTrade2019.json";
import VirusData from "~/assets/virus-grouped.json";
import * as d3 from "d3";
export default {
  name: "TradeWithVirus",
  props: ["selectedVirus"],
  data() {
    return {
      allTrade: allTrade2019,
      VirusData: VirusData,
      updatedVirusName: null,
      updatedVirusData: null,
      allGenusWithUpdatedVirus: null,
      tradedGenusWithVirus: null
     
    };
  },
  mounted() {
      this.updateData();
      this.updateSelectedVirusData();
      this.parseData();
      this.findGenusTraded()
  },
  methods: {
    updateData() {
       this.updatedVirusName = this.$props.selectedVirus;
       
    },
    updateSelectedVirusData() {
        this.updatedVirusData = VirusData[this.updatedVirusName]
    },
    parseData() {
        let allGenus = []
        let selectedVirusData = this.updatedVirusData;
        for(let family in selectedVirusData) {
            selectedVirusData[family].genus.forEach(e => {
                allGenus.push((e.genusName))
            })
        }
        this.allGenusWithUpdatedVirus = allGenus;
    },
    findGenusTraded() {
        let allTraded = this.allTrade;
        let allGenusWithThisVirus = this.allGenusWithUpdatedVirus;
        // console.log(allTraded)
        let tradedWithVirus = [];
        allTraded.forEach(e => {
            let tradedGenus = e.genusName;
            allGenusWithThisVirus.forEach(g => {
                if (tradedGenus === g) {
                    tradedWithVirus.push(tradedGenus)
                }
            })

        });
        this.tradedGenusWithVirus = tradedWithVirus;
    }

  },
  watch: {
      selectedVirus() {
          this.updateData();
          this.updateSelectedVirusData();
          this.parseData();
          this.findGenusTraded();
      }
  }
};
</script>
<style scoped>
p {
  color: white;
}

.main {
  padding-top: 50px;
  padding-bottom: 50px;
}
</style>
