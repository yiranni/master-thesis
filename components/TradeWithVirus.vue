<template>
  <div class="main">
    <div id="bubbleChart">
      <svg ref="quantityBubbles" id="qantityBubbles" />
    </div>
  </div>
</template>

<script>
import * as d3 from "d3";
import allTrade from "~/assets/data/allTrade.json";
import VirusData from "~/assets/virus-grouped.json";
import reservoirs from "~/assets/data/reservoirs.json";
export default {
  name: "TradeWithVirus",
  data() {
    return {
      allTrade,
      VirusData: VirusData,
      VirusArray: null,
      allReservoirs: reservoirs,
      width: 1000,
      height: 400
    };
  },
  mounted() {
    this.parseData();
    // this.getAllGenera();
    this.createBubble();
  },
  methods: {
    parseData() {
      let allVirus = [];
      let data = this.VirusData;
      for (let virus in data) {
        let obj = {};
        obj.virusName = virus;
        let allFamily = data[virus];
        let allGenus = [];
        for (let family in allFamily) {
          let genera = allFamily[family].genus;
          genera.forEach(e => {
            allGenus.push(e);
          });
        }
        obj.reservoirs = allGenus;
        allVirus.push(obj);
      }
      this.VirusArray = allVirus;
    },

    createBubble() {
      const data = this.allTrade;
      const reservoirdata = this.allReservoirs;
      const chartContainer = this.$refs.quantityBubbles;
      let containerWidth = this.width;
      let containerHeight = this.height;

      const pieChartOuterRadius = 150;
      const pieChartInnerRadius = 130;

      /* compare trade with reservoirs and non-reservoir */

      let export_reported_total = 0;
      let import_reported_total = 0;
      data.forEach(t => {
        if (t.Exporterreportedquantity.length > 0) {
          let quant = parseInt(t.Exporterreportedquantity);

          export_reported_total += quant;
        }
        if (t.Importerreportedquantity.length > 0) {
          let quant = parseInt(t.Importerreportedquantity);

          import_reported_total += quant;
        }
      });

      console.log(import_reported_total);

      let export_reported_with_anyD = 0;

      data.forEach(t => {
        let genus = t.Genus;
        let amount = t.Exporterreportedquantity;
        reservoirdata.forEach(r => {
          if (genus === r.genusName) {
            if (amount.length > 0) {
              export_reported_with_anyD += parseInt(amount);
            }
          }
        });
      });

      let import_reported_with_anyD = 0;

      data.forEach(t => {
        let genus = t.Genus;
        let amount = t.Importerreportedquantity;
        reservoirdata.forEach(r => {
          if (genus === r.genusName) {
            if (amount.length > 0) {
              import_reported_with_anyD += parseInt(amount);
            }
          }
        });
      });

      let exportCompare = {
        reservoir: export_reported_with_anyD,
        "non reservoir": export_reported_total - export_reported_with_anyD
      };

      let importCompare = {
        reservoir: import_reported_with_anyD,
        "non reservoir": import_reported_total - import_reported_with_anyD
      };

      /* Chart */
      /* Pie Chart Global Variables */
      const chart = d3
        .select(chartContainer)
        .attr("width", containerWidth)
        .attr("height", containerHeight)
        .attr("transform", `translate(0,0)`);

      const color = d3
        .scaleOrdinal()
        .domain(exportCompare)
        .range(["#b60b0b", "#A6A3A3"]);

      const pie = d3.pie().value(function(d) {
        return d.value;
      });

      function handlePieMouseOver() {
        d3.select(this).attr("stroke", "white");
        let className = d3.select(this).attr("class");
        let id = d3.select(this).attr("id");
        let value = d3.select(this).attr("value");
        d3.select(`text.${className}.function`).text(function(d) {
          return id;
        });
        d3.select(`text.${className}.number`).text(function(d) {
          return value;
        });
      }

      function handlePieMouseOut() {
        d3.select(this).attr("stroke", "none");
      }

      /* Pie Chart Import */
      const import_ready = pie(d3.entries(importCompare));

      const importAll = chart.append("g");

      const importArc = importAll
        .selectAll(".import")
        .data(import_ready)
        .enter()
        .append("path")
        .attr("class", "importInfo")
        .attr("id", function(d) {
          return d.data.key;
        })
        .attr("value", function(d) {
          return d.data.value;
        })
        .attr(
          "d",
          d3
            .arc()
            .innerRadius(pieChartInnerRadius)
            .outerRadius(pieChartOuterRadius)
        )
        .attr("fill", function(d) {
          return color(d.data.key);
        })
        .attr(
          "transform",
          `translate(${pieChartOuterRadius + 100}, ${pieChartOuterRadius + 50})`
        )
        .on("mouseover", handlePieMouseOver)
        .on("mouseout", handlePieMouseOut);

      const importLabelNumber = importAll
        .append("text")
        .attr("class", "importInfo number")
        .text(import_reported_total)
        .attr("fill", "white")
        .attr(
          "transform",
          `translate(${pieChartOuterRadius + 100}, ${pieChartOuterRadius + 20})`
        )
        .attr("text-anchor", "middle")
        .attr("font-size", "24")
        .attr("display", "block");

      const importLabel = importAll
        .append("text")
        .attr("class", "importInfo function")
        .text("imported")
        .attr("fill", "white")
        .attr(
          "transform",
          `translate(${pieChartOuterRadius + 100}, ${pieChartOuterRadius + 60})`
        )
        .attr("text-anchor", "middle")
        .attr("font-size", "20");

      /* Pie Chart Export */

      const export_ready = pie(d3.entries(exportCompare));

      const exportAll = chart.append("g");

      const exportArc = exportAll
        .selectAll(".export")
        .data(export_ready)
        .enter()
        .append("path")
        .attr("class", "exportInfo")
        .attr("id", function(d) {
          return d.data.key;
        })
        .attr("value", function(d) {
          return d.data.value;
        })
        .attr(
          "d",
          d3
            .arc()
            .innerRadius(pieChartInnerRadius)
            .outerRadius(pieChartOuterRadius)
        )
        .attr("fill", function(d) {
          return color(d.data.key);
        })
        .attr(
          "transform",
          `translate(${pieChartOuterRadius + 600}, ${pieChartOuterRadius + 50})`
        )
        .on("mouseover", handlePieMouseOver)
        .on("mouseout", handlePieMouseOut);

      const exportLabelNumber = exportAll
        .append("text")
        .attr("class", "exportInfo number")
        .text(export_reported_total)
        .attr("fill", "white")
        .attr(
          "transform",
          `translate(${pieChartOuterRadius + 600}, ${pieChartOuterRadius + 20})`
        )
        .attr("text-anchor", "middle")
        .attr("font-size", "24");

      const exportLabel = exportAll
        .append("text")
        .attr("class", "exportInfo function")
        .text("exported")
        .attr("fill", "white")
        .attr(
          "transform",
          `translate(${pieChartOuterRadius + 600}, ${pieChartOuterRadius + 60})`
        )
        .attr("text-anchor", "middle")
        .attr("font-size", "20");
    }
  },
  watch: {}
};
</script>
<style scoped>
p {
  color: white;
}

.main {
  /* padding-top: 50px; */
  padding-bottom: 50px;
  font-family: Arial, Helvetica, sans-serif;
}

div.tooltip {
  position: absolute;
  text-align: center;
  left: 200;
  top: 0;
  width: 60px;
  height: 28px;
  padding: 2px;
  font: 12px sans-serif;
  background: lightsteelblue;
  border: 0px;
  border-radius: 8px;
  pointer-events: none;
}
</style>
