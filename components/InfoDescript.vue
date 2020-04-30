<template>
  <div>
    <p class="virusName">{{selectedVirusDescrip}}</p>
  </div>
</template>

<script>
export default {
  props: { selectedVirusDescrip: String },
  name: "InfoDescript",
  data() {
    return {};
  }
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
</style>
