<template>
  <div class="main">
    <h1>About</h1>
    <p>This is an about page</p>
    <!-- <svg class="figure"> -->
    <!-- <g > -->
    <Virus width="500"/>
    <!-- </g> -->
    <!-- </svg> -->
  </div>
</template>

<script>
import Virus from "~/components/Virus.vue";
export default {
  name: "Landing",
  components: {
    Virus
  }
};
</script>

<style scoped>
.main {
  width: 100vw;
  height: 100vh;
  background-color: #313437;
  z-index: 0;
  padding-left: 200px;
}

h1 {
  margin-left: 200px;
}

.figure {
  margin-left: 200px;
}


</style>
