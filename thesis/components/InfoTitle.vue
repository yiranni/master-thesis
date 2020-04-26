<template>
  <div>
    <h1 class="virusName">{{selectedVirusName}}</h1>
  </div>
</template>

<script>
export default {
  props: { selectedVirusName: String },
  name: "InfoTitle",
  data() {
    return {};
  }
};
</script>

<style scoped>
h1 {
  color: white;
  margin-left: 40px;
  font-size: 20px;
}
</style>
