<template>
  <div>
    <p v-for="f in allFamily" :key="f.familyName">
        {{f.familyName}}
    </p>
  </div>
</template>

<script>
import * as d3 from "d3";
import VirusData from "~/assets/virus-grouped.json";
export default {
  props: ["allFamily"],
  name: "Title",
  data() {
    return {
      VirusData: VirusData,
      selectedVirus: ""
    };
  },
  mounted() {
    this.setup();
  },
  methods: {
    setup() {
      console.log(this.selectedVirus);
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
</style>
