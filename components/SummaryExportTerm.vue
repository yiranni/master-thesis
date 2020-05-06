<template>
  <div class="main">
    <svg ref="bar" />
  </div>
</template>

<script>
import exportByTerm from "~/assets/data/exportByTerm.json";
import * as d3 from "d3";
export default {
  name: "SummaryExportTerm",
  props: [],
  data() {
    return {
      exportByTerm: exportByTerm,
      width: 700,
      height: 200
    };
  },
  mounted() {
    this.createBarChart();
  },
  methods: {
    createBarChart() {
      const exportTermData = this.exportByTerm;
      const barChartContainer = this.$refs.bar;
      let containerWidth = this.width;
      let containerHeight = this.height;
      let containerMarginRight = 150;
      let containerMarginLeft = 120;

      const barChart = d3
        .select(barChartContainer)
        .attr("width", containerWidth)
        .attr("height", containerHeight);

      const yScale = d3
        .scaleBand()
        .range([0, containerHeight])
        .padding(0.4);
      const xScale = d3.scaleLinear().range([0, containerWidth - containerMarginRight- containerMarginLeft]);

      yScale.domain(
        exportTermData.map(function(d) {
          return d.Term;
        })
      );

      xScale.domain([
        0,
        d3.max(exportTermData, function(d) {
          return d.Amount;
        })
      ]);
      const bars = barChart
        .selectAll(".bar")
        .data(exportTermData)
        .enter()
        .append("g");

      bars
        .append("rect")
        .attr("class", "bar")
        .attr("fill", "#FF6496")
        .attr("x", function(d) {
          return 0;
        })
        .attr("height", yScale.bandwidth())
        .attr("y", function(d) {
          return yScale(d.Term);
        })
        .attr("width", function(d) {
          return xScale(d.Amount);
        })
        .attr("transform", `translate(${containerMarginLeft + 10},0)`);

      bars
        .append("g")
        .attr("class", "axis axisY")
        .call(d3.axisLeft(yScale))
        .attr("transform", `translate(${containerMarginLeft},0)`);

        bars
          .append("text")
          .attr("class", "label")
          .attr("y", function(d) {
            
            return yScale(d.Term) + yScale.bandwidth()/2 + 6
          })
          .attr("x", function(d) {
            return xScale(d.Amount);
          })
          .text(function(d) {
            return d.Amount;
          })
          .attr('text-anchor', 'start')
          .attr('fill', 'white')
          .attr("transform", `translate(${containerMarginLeft + 16},0)`);

      d3.selectAll(".domain").remove();
      d3.selectAll(".tick line").remove();
      d3.selectAll(".tick text")
        .attr("font-size", "18")
        .attr("fill", "#FF6496");
    }
  }
};
</script>
<style scoped>
p {
  color: white;
}

.main {
  padding-top: 50px;
  padding-bottom: 50px;
}
</style>
