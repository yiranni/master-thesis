<template>
  <div>
    <p>{{importData(trackAnimal)}}</p>
  </div>
</template>

<script>
import TradingData from "~/assets/data/trade/Acinonyx.json";

export default {
  name: "Purpose",
  props: ["trackAnimal"],
  data() {
    return {
      tradeInfo: null
    };
  },
  mounted() {
    // this.getData();
  },
  computed: {},
  methods: {
    importData(genusName) {
      let data = import(`../assets/data/trade/${genusName}.json`);
      let promise = new Promise((res, rej) => {
          res(data)
      })
      
      promise.then(val => {
          console.log(val)
          return val[0]
      });
      
    }
  }
};
</script>

<style scoped>
p {
  color: white;
  font-size: 16px;
}
</style>
