<template>
  <div class="main">
    <svg ref="partyJoin" />
  </div>
</template>

<script>

import partyData from "~/assets/data/parties.json";
import * as d3 from "d3";
export default {
  name: "PartyJoin",
  props: [],
  data() {
    return {

      partyData: partyData,
      width: 700,
      height: 300
    };
  },
  mounted() {
    this.createLineChart();
  },
  methods: {
    createLineChart() {
      let height = this.height;
      let width = this.width;

      const data = this.partyData;
      const lineChartContainer = this.$refs.partyJoin;
      let containerMarginTop = 20;
      let containerMarginBottom = 40;
      let containerMarginRight = 0;
      let containerMarginLeft = 50;
      let containerWidth = width - containerMarginLeft - containerMarginRight;
      let containerHeight = height - containerMarginTop - containerMarginBottom;

      const nestedData = d3
        .nest()
        .key(function(d) {
          return d.Dateofjoiningsortdescending;
        })
        .entries(data);

      let cumulatedByDate = [];
      let runningAmount = 0;
      for (let i = 0; i < nestedData.length; i++) {
        let obj = {};
        obj.date = nestedData[i].key;
        runningAmount = runningAmount + nestedData[i].values.length;
        obj.runningAmount = runningAmount;
        cumulatedByDate.push(obj);
      }

      const lineChart = d3
        .select(lineChartContainer)
        .attr(
          "width",
          containerWidth + containerMarginLeft + containerMarginLeft
        )
        .attr(
          "height",
          containerHeight + containerMarginTop + containerMarginBottom
        )
        .append("g")
        .attr(
          "transform",
          "translate(" + containerMarginLeft + "," + containerMarginTop + ")"
        );

      let parseTime = d3.timeParse("%d/%m/%Y");

      cumulatedByDate.forEach(function(d) {
        d.date = parseTime(d.date);
        d.runningAmount = d.runningAmount;
      });

      const x = d3.scaleTime().range([0, containerWidth]);
      const y = d3.scaleLinear().range([containerHeight, 0]);

      x.domain(
        d3.extent(cumulatedByDate, function(d) {
          return d.date;
        })
      );

      y.domain([
        0,
        d3.max(cumulatedByDate, function(d) {
          return d.runningAmount;
        })
      ]);

      lineChart
        .append("g")
        .attr("class", "xAxis")
        .attr("transform", `translate(0, ${containerHeight})`)

        .style("color", "#A6A3A3")
        .style("font-size", "18px")
        .call(d3.axisBottom(x).tickSize(0).tickPadding(20).ticks(6));

      lineChart
        .append("g")
        .attr("class", "yAxis")
        .style("color", "#A6A3A3")
        .style("font-size", "18px")
        .call(d3.axisLeft(y).tickSize(0).tickPadding(20).ticks(5));

      const valueLine = d3
        .line()
        .x(function(d) {
          return x(d.date);
        })
        .y(function(d) {
          return y(d.runningAmount);
        });

      lineChart
        .append("path")
        .data([cumulatedByDate])
        .attr("class", "line")
        .attr("d", valueLine)
        .attr("fill", "none")
        .attr("stroke-width", "3")
        .attr("stroke", "#b60b0b");
      

      d3.selectAll(".yAxis>.tick>line").remove()
      d3.selectAll(".xAxis>.tick>line").remove()
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
