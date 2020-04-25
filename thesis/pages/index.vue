<template>
  <div class="model">
    <Nav />
    <Explore />
    <Info />
    <!-- <Intro /> -->
    <!-- <Header>Demo</Header> -->
    <!-- <Virus3D /> -->
  </div>
</template>

<script>
import Nav from "~/components/Nav.vue";
import Explore from "~/components/Explore.vue";
import Info from "~/components/Info.vue";

export default {
  components: {
    Nav,
    Explore,
    Info
    // Intro,
    // Header,
    // Virus3D
    // BarChart
  }
};
</script>

<style>
</style>
