<template>
  <div class="main">
    <svg ref="elephant" />
  </div>
</template>

<script>
import * as d3 from "d3";
export default {
  name: "Elephant",
  props: [],
  data() {
    return {
      width: 768,
      height: 768
    };
  },
  mounted() {
    this.createLineChart();
  },
  methods: {
    createLineChart() {
      let height = this.height;
      let width = this.width;

      const container = this.$refs.elephant;

      const chart = d3
        .select(container)
        .attr("width", width)
        .attr("height", height)
        .append("g");

      let data = [];
      for (let i = 0; i < 1024; i++) {
        data.push("killed");
      }

      chart
        .selectAll(".elephant")
        .data(data)
        .enter()
        .append("rect")
        .attr("width", "20")
        .attr("height", "20")
        .attr("x", function(d, i) {
          return (i % 32) * 24;
        })
        .attr("y", function(d, i) {
          return Math.floor(i / 32) * 24;
        })
        .attr("fill", "#b60b0b");


        chart.append("rect").attr("width", "20")
        .attr("height", "20").attr("fill", "#A6A3A3")
    }

    
  }
};
</script>
<style scoped>
p {
  color: white;
}

.main {
    margin: 0;
    padding: 0
}
</style>
