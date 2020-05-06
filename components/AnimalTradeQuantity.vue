<template>
  <div class="main">
    <svg ref="speciesQuantity" />
  </div>
</template>

<script>
import AnimalTradeQuantity from "~/assets/data/allGenusQuantity.json";
import * as d3 from "d3";
export default {
  name: "SummaryImportCountry",
  props: [],
  data() {
    return {
      AnimalTradeQuantity: AnimalTradeQuantity,
      width: 700,
      height: 200
    };
  },
  mounted() {
    this.createBarChart();
  },
  methods: {
    createBarChart() {
      const allData = this.AnimalTradeQuantity;
      let top5 = allData
        .sort(function(a, b) {
          return d3.descending(
            +a.Exporter_reported_quantity,
            +b.Exporter_reported_quantity
          );
        })
        .slice(0, 3);
      const barChartContainer = this.$refs.speciesQuantity;
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
      const xScale = d3
        .scaleLinear()
        .range([
          0,
          containerWidth - containerMarginRight - containerMarginLeft
        ]);

      yScale.domain(
        top5.map(function(d) {
          return d.genus_name;
        })
      );

      xScale.domain([
        0,
        d3.max(top5, function(d) {
          return d.Exporter_reported_quantity;
        })
      ]);

      const bars = barChart
        .selectAll(".bar")
        .data(top5)
        .enter()
        .append("g");

      const bar = bars.append("g").attr("class", "bar");

      const barRect = bar
        .append("rect")
        .attr("class", "genusRect")
        .attr("id", function(d) {
          return d.genus_name + "rect";
        })
        .attr("fill", "#FF6496")
        .attr("x", function(d) {
          return 0;
        })
        .attr("height", yScale.bandwidth())
        .attr("y", function(d) {
          return yScale(d.genus_name);
        })
        .attr("width", function(d) {
          return xScale(d.Exporter_reported_quantity);
        })
        .attr("transform", `translate(${containerMarginLeft + 10},0)`);

      bars
        .append("g")
        .attr("class", "axis axisY")
        .call(d3.axisLeft(yScale))
        .attr("transform", `translate(${containerMarginLeft},0)`);

      const barLabel = bar
        .append("text")
        .attr("class", "importCountry")
        .attr("id", function(d) {
          return d.genus_name + "label";
        })
        .attr("y", function(d) {
          return yScale(d.genus_name) + yScale.bandwidth() / 2 + 6;
        })
        .attr("x", function(d) {
          return xScale(d.Exporter_reported_quantity);
        })
        .text(function(d) {
          return (
            Math.round(d.Exporter_reported_quantity) 
          );
        })
        .attr("text-anchor", "start")
        .attr("fill", "white")
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
  padding-top: 30px;
  padding-bottom: 0px;
}
</style>
