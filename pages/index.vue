<template>
  <div class="main">
    <Landing v-bind:VirusOptions="VirusOptions"/>
  </div>
</template>

<script>
import VirusData from "~/assets/virus-grouped.json";
import Landing from "~/components/Landing.vue";
export default {
  components: {
    Landing
  },
  data() {
    return {
      VirusData: VirusData,
      VirusOptions: []
    };
  },
  mounted() {
    this.getVirusOptions();
  },
  methods: {
    getVirusOptions() {
      const virusData = VirusData;
      Object.keys(virusData).forEach(v => {
        let obj = {};
        obj.text = v;
        obj.value = v;
        obj.virusName = v;
        obj.genusCount = 0;
        Object.keys(virusData[v]).forEach(f => {
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
          let allGenus = virusData[v][f].genus;
          obj.genusCount += allGenus.length;
        });

        // console.log(obj);
        this.VirusOptions.push(obj);
      });
    }
  }
};
</script>

<style scoped>

</style>
