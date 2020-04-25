<template>
  <div class="section">
    <h1>{{ virusName }}</h1>
    <!-- <p :virusName="virusName"></p> -->
  </div>
</template>

<script>
import Explore from "~/components/Explore.vue";
export default {
  // props: ["virusName"],
  // props: ['option.value'],
  name: "Info",
  data() {
    return {
      virusName: "Virus"
    };
  }
};
</script>

<style scoped>
.section {
  height: 100%;
  width: 540px;
  position: fixed;
  z-index: 1;
  top: 0;
  right: 0;
  background-color: #000000;
  overflow-x: hidden;
  /* padding-top: 20px; */
  padding-top: 80px;
}

h1 {
  color: white;
  margin-left: 30px;
  font-size: 20px;
}
p {
  color: white;
}
</style>
