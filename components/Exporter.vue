<template>
  <div class="main">
    <svg ref="exportCountry" />
  </div>
</template>

<script>
import AnimalTrade from "~/assets/data/AcinonyxTopExporter.json";
import * as d3 from "d3";
export default {
  name: "Exporter",
  props: [],
  data() {
    return {
      Top5Exporter: AnimalTrade,
      width: 240,
      height: 200
    };
  },
  mounted() {
    this.createBarChart();
  },
  methods: {
    createBarChart() {
      const exportCountryData = this.Top5Exporter;
      const barChartContainer = this.$refs.exportCountry;
      let containerWidth = this.width;
      let containerHeight = this.height;
      let containerMarginRight = 60;
      let containerMarginLeft = 30;

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
        exportCountryData.map(function(d) {
          return d.Country;
        })
      );

      xScale.domain([
        0,
        d3.max(exportCountryData, function(d) {
          // console.log(d)
          return d.Amount;
        })
      ]);
      const bars = barChart
        .selectAll(".exporterbar")
        .data(exportCountryData)
        .enter()
        .append("g");

      const bar = bars.append("g").attr("class", "importerbar");

      const barRect = bar
        .append("rect")
        .attr("class", "exporterbarRect")
        .attr("id", function(d) {
          return d.Country + "rect";
        })
        .attr("fill", "#FF6496")
        .attr("x", function(d) {
          return 0;
        })
        .attr("height", yScale.bandwidth())
        .attr("y", function(d) {
          return yScale(d.Country);
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
        .attr('class', 'exportCountry')
        .attr("id", function(d) {
          return d.Country + "label";
        })
        .attr("y", function(d) {
          return yScale(d.Country) + yScale.bandwidth() / 2 + 6;
        })
        .attr("x", function(d) {
          return xScale(d.Amount);
        })
        .text(function(d) {
          return d.Amount 
        })
        .attr("text-anchor", "start")
        .attr("fill", "white")
        .attr('font-size', '14')
        .attr("transform", `translate(${containerMarginLeft + 16},0)`);

      d3.selectAll(".domain").remove();
      d3.selectAll(".tick line").remove();
      d3.selectAll(".tick text")
        .attr("font-size", "14")
        .attr("fill", "#FF6496");

      // d3.selectAll(".importerbarRect").on("mouseover", handleMouseOver);
      // d3.selectAll(".importerbarRect").on("mouseout", handleMouseOut);

      // function handleMouseOver(d) {
      //   let thisLabel = ".importCountry#" + d.Country + "label";
      //   console.log(thisLabel);
      //   d3.select(thisLabel).text(function(d) {
      //     return d.Amount;
      //   });
      // }

      // function handleMouseOut(d) {
      //   let thisLabel = ".importCountry#" + d.Country + "label";
      //   d3.select(thisLabel).text(function(d) {
      //     return Math.round((d.Amount / 61224) * 1000) / 10 + "%";
      //   });
      // }
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
