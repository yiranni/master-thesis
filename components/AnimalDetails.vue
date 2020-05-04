<template>
  <div class="container">
    <h1>{{selectedAnimalCommonName}}</h1>
    <h2>Genus: {{selectedAnimal}}</h2>
    <Purpose v-bind:trackAnimal="trackAnimal" :dataPath="dataPath"/>
  </div>
</template>

<script>
import AnimalName from "~/assets/animalName.json";
import Purpose from "~/components/Purpose.vue";
export default {
  name: "AnimalDetails",
  components: {
    Purpose
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
      this.dataPath = `~/assets/data/trade/${matched.genusName}.json`
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
</style>
