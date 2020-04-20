<template>
  <div class="model">
    <Intro />
    <!-- <Header>Demo</Header> -->
    <Virus3D />
  </div>
</template>

<script>
import Intro from "~/components/Intro.vue";
// import Header from "~/components/Header.vue";
import Virus3D from "~/components/Virus3D.vue";
// import BarChart from '~/components/BarChart.vue'

export default {
  components: {
    Intro,
    // Header,
    Virus3D
    // BarChart
  }
};
</script>

<style>

</style>
