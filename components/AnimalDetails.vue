<template>
  <div class="container">
    <h1>{{matchName(selectedAnimal)}}</h1>
    <h2>Genus: {{selectedAnimal}}</h2>
  </div>
</template>

<script>
import AnimalName from "~/assets/animalName.json";
export default {
  name: "AnimalDetails",
  props: ["selectedAnimal"],
  data() {
    return {};
  },
  methods: {
    matchName(genus) {
      let nameLibray = AnimalName;
      let matched = nameLibray.find(({ genusName }) => genusName === genus);
      return matched.commonName;
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
