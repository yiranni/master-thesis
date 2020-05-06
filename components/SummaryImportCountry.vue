<template>
  <div class="main">
    <svg ref="importCountryBar" />
  </div>
</template>

<script>
import Top5Importer from "~/assets/data/Top5Importer.json";
import * as d3 from "d3";
export default {
  name: "SummaryImportCountry",
  props: [],
  data() {
    return {
      Top5Importer: Top5Importer,
      width: 700,
      height: 300
    };
  },
  mounted() {
    this.createBarChart();
  },
  methods: {
    createBarChart() {
      const importCountryData = this.Top5Importer;
      const barChartContainer = this.$refs.importCountryBar;
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
        importCountryData.map(function(d) {
          return d.Country_Full;
        })
      );

      xScale.domain([
        0,
        d3.max(importCountryData, function(d) {
          // console.log(d)
          return d.Amount;
        })
      ]);
      const bars = barChart
        .selectAll(".bar")
        .data(importCountryData)
        .enter()
        .append("g");

      const bar = bars.append("g").attr("class", "bar");

      const barRect = bar
        .append("rect")
        .attr("class", "importbarRect")
        .attr("id", function(d) {
          return d.Country + "rect";
        })
        .attr("fill", "#FF6496")
        .attr("x", function(d) {
          return 0;
        })
        .attr("height", yScale.bandwidth())
        .attr("y", function(d) {
          return yScale(d.Country_Full);
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

      const barLabel = bar
        .append("text")
        .attr('class', 'importCountry')
        .attr("id", function(d) {
          return d.Country + "label";
        })
        .attr("y", function(d) {
          return yScale(d.Country_Full) + yScale.bandwidth() / 2 + 6;
        })
        .attr("x", function(d) {
          return xScale(d.Amount);
        })
        .text(function(d) {
          return Math.round((d.Amount / 61224) * 1000) / 10 + "%";
        })
        .attr("text-anchor", "start")
        .attr("fill", "white")
        .attr("transform", `translate(${containerMarginLeft + 16},0)`);

      d3.selectAll(".domain").remove();
      d3.selectAll(".tick line").remove();
      d3.selectAll(".tick text")
        .attr("font-size", "18")
        .attr("fill", "#FF6496");

      d3.selectAll(".importbarRect").on("mouseover", handleMouseOver);
      d3.selectAll(".importbarRect").on("mouseout", handleMouseOut);

      function handleMouseOver(d) {
        let thisLabel = ".importCountry#" + d.Country + "label";
        console.log(thisLabel);
        d3.select(thisLabel).text(function(d) {
          return d.Amount;
        });
      }

      function handleMouseOut(d) {
        let thisLabel = ".importCountry#" + d.Country + "label";
        d3.select(thisLabel).text(function(d) {
          return Math.round((d.Amount / 61224) * 1000) / 10 + "%";
        });
      }
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
