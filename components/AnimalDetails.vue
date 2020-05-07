<template>
  <div class="container">
    <h1>{{selectedAnimalCommonName}}</h1>
    <h2>Genus: {{selectedAnimal}}</h2>
    <Purpose v-bind:trackAnimal="trackAnimal" :dataPath="dataPath" />
    <div class="country">
      <div class="import">
        <p>Top 5 Import Countries</p>
        <Importer />
      </div>
      <div class="export">
        <p>Top 5 Export Countries</p>
        <Exporter />
      </div>
    </div>
  </div>
</template>

<script>
import AnimalName from "~/assets/animalName.json";
import Purpose from "~/components/Purpose.vue";
import Importer from "~/components/Importer.vue";
import Exporter from "~/components/Exporter.vue";
export default {
  name: "AnimalDetails",
  components: {
    Purpose,
    Importer,
    Exporter
  },
  props: ["selectedAnimal"],
  data() {
    return {
      selectedAnimalCommonName: "",
      trackAnimal: "",
      dataPath: ""
    };
  },
  mounted() {
    this.matchName();
  },
  methods: {
    matchName() {
      let genus = this.$props.selectedAnimal;
      let nameLibray = AnimalName;
      let matched = nameLibray.find(({ genusName }) => genusName === genus);
      this.selectedAnimalCommonName = matched.commonName;
      this.trackAnimal = matched.genusName;
      this.dataPath = `~/assets/data/trade/${matched.genusName}.json`;
    }
  },
  watch: {
    selectedAnimal() {
      this.matchName();
    }
  }
};
</script>

<style scoped>
.container {
  margin-left: 40px;
}
h1 {
  color: white;

  font-size: 20px;
}

h2 {
  color: #838383;
  font-weight: normal;
  font-size: 18px;
  font-style: italic;
  padding-top: 20px;
}

.country {
  color: white;
  display: flex;
  padding-top: 100px;
  font-size: 20px;
}

.country div {
  width: 240px;
}
</style>
