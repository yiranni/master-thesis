<template>
  <div class="main">

    <svg ref="virusBar" />
  </div>
</template>

<script>
import allTrade from "~/assets/data/allTrade.json";
import VirusData from "~/assets/virus-grouped.json";
import reservoirs from "~/assets/data/reservoirs.json";
import * as d3 from "d3";
export default {
  name: "TradingDetails",
  props: [],
  data() {
    return {
      allTrade: allTrade,
      VirusData: VirusData,
      reservoirs: reservoirs,
      VirusArray: null,
      width: 1000,
      height: 1000,
      virusGrouped: null,
      genusGrouped: null,
      virusTradeAmount: null,
      exportSelected: false,
      importSelected: false,
      counter: 0
    };
  },
  mounted() {
    this.parseData();
    this.getGenusGroupedData();
    this.getViruGroupedData();
    this.calculateTrade();
    this.createBarChart();
  },
  methods: {
    parseData() {
      let tradeData = this.allTrade;
      let virusData = this.VirusData;
      let reservoirsData = this.reservoirs;

      let allVirus = [];
      let data = this.VirusData;
      for (let virus in virusData) {
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
    getGenusGroupedData() {
      const virusData = this.VirusArray;

      const tradeData = this.allTrade;

      let genusGroupedTradeDataMap = new Map();

      tradeData.forEach(t => {
        let genusName = t.Genus;
        let Importerreportedquantity;
        let Exporterreportedquantity;

        if (t.Importerreportedquantity.length > 0) {
          Importerreportedquantity = parseInt(t.Importerreportedquantity);
        } else {
          Importerreportedquantity = 0;
        }

        if (t.Exporterreportedquantity > 0) {
          Exporterreportedquantity = parseInt(t.Exporterreportedquantity);
        } else {
          Exporterreportedquantity = 0;
        }

        if (!genusGroupedTradeDataMap.has(genusName)) {
          genusGroupedTradeDataMap.set(genusName, {
            Exporterreportedquantity: Exporterreportedquantity,
            Importerreportedquantity: Importerreportedquantity
          });
        } else {
          // console.log(genusGroupedTradeDataMap.get(genusName).Exporterreportedquantity)
          genusGroupedTradeDataMap.get(
            genusName
          ).Exporterreportedquantity += Exporterreportedquantity;
          genusGroupedTradeDataMap.get(
            genusName
          ).Importerreportedquantity += Importerreportedquantity;
        }
      });

      let genusGroupedTradeData = Array.from(genusGroupedTradeDataMap);
      this.genusGrouped = genusGroupedTradeData;
    },
    getViruGroupedData() {
      let virusData = this.VirusArray;
      let genusGroupedTradeData = this.genusGrouped;
      let virusDataWithTrading = [];
      for (let i = 0; i < virusData.length; i++) {
        let obj = {};
        obj.virusName = virusData[i].virusName;
        let virusreservoirs = [];
        let reservoirs = virusData[i].reservoirs;
        for (let j = 0; j < reservoirs.length; j++) {
          let genusName = reservoirs[j].genusName;
          let thisReservoir = {};
          thisReservoir.genusName = genusName;
          thisReservoir.commonName = reservoirs[j].commonName;
          thisReservoir.direct = reservoirs[j].direct;
          for (let g = 0; g < genusGroupedTradeData.length; g++) {
            if (thisReservoir.genusName === genusGroupedTradeData[g][0]) {
              thisReservoir.trade = genusGroupedTradeData[g][1];
            }
            // console.log(thisReservoir);
          }
          // console.log(thisReservoir)
          virusreservoirs.push(thisReservoir);
        }
        obj.reservoirs = virusreservoirs;
        virusDataWithTrading.push(obj);
      }

      this.virusGrouped = virusDataWithTrading;
    },
    calculateTrade() {
      let virusData = this.virusGrouped;

      let virusTrade = [];

      for (let i = 0; i < virusData.length; i++) {
        // console.log(virusDlet virusTrade = [];
        let obj = {};
        let directImport = 0;
        let indirectImport = 0;
        let directExport = 0;
        let indirectExport = 0;

        obj.virusName = virusData[i].virusName;

        let reservoirs = virusData[i].reservoirs;

        reservoirs.forEach(r => {
          let trade = { ...r.trade };
          let importAmount = trade.Importerreportedquantity;
          let exportAmount = trade.Exporterreportedquantity;
          let influence = r.direct;
          let name = r.genusName;

          if (influence === true) {
            directImport += importAmount;
            directExport += exportAmount;
          } else {
            //  console.log(name + ":" + exportAmount);
            indirectImport += importAmount;
            indirectExport += exportAmount;
          }

          obj.directImport = directImport;
          obj.directExport = directExport;
          obj.indirectImport = indirectImport;
          obj.indirectExport = indirectExport;
        });

        virusTrade.push(obj);
      }

      this.virusTradeAmount = virusTrade;
    },
    createBarChart() {
      const data = this.virusTradeAmount;
      let sortedExportAmount = data.sort((a, b) =>
        d3.descending(
          a.directExport + a.indirectExport,
          b.directExport + b.indirectExport
        )
      );

      console.log(sortedExportAmount);
      const barChartContainer = this.$refs.virusBar;
      let containerWidth = this.width;
      let containerHeight = this.height;
      let containerMarginRight = 40;
      let containerMarginLeft = 210;

      const barChart = d3
        .select(barChartContainer)
        .attr("width", containerWidth)
        .attr("height", containerHeight);

      const yScale = d3
        .scaleBand()
        .range([0, containerHeight - 100])
        .padding(0.4);
      const xScale = d3
        .scaleLinear()
        .range([
          containerMarginLeft,
          (containerWidth - containerMarginRight) / 2,
          containerWidth - containerMarginRight
        ]);

      yScale.domain(
        sortedExportAmount.map(function(d) {
          return d.virusName;
        })
      );

      xScale.domain([0, 1000, 53536]);
      const bars = barChart
        .selectAll(".virusbar")
        .data(sortedExportAmount)
        .enter()
        .append("g");

      const reservoirCircles = bars
        .append("circle")
        .attr("class", "totalexport")
        .attr("fill", "none")
        .attr("stroke", "#b60b0b")
        .attr("stroke-width", "3")
        .attr("r", "14")
        .attr("cx", function(d) {
          return xScale(d.directExport + d.indirectExport);
        })
        .attr("cy", function(d) {
          return yScale(d.virusName) + yScale.bandwidth() / 2;
        })
        .attr("transform", `translate(16,40)`);

      const totalCircles = bars
        .append("circle")
        .attr("class", "totalexport")
        .attr("fill", "none")
        .attr("stroke", "#a6a3a3")
        .attr("stroke-width", "3")
        .attr("r", "14")
        .attr("cx", function(d) {
          return xScale(53536);
        })
        .attr("cy", function(d) {
          return yScale(d.virusName) + yScale.bandwidth() / 2;
        })
        .attr("transform", `translate(16,40)`);

      const connectLine = bars
        .append("line")
        .attr("x1", function(d) {
          return xScale(d.directExport + d.indirectExport) + 14;
        })
        .attr("y1", function(d) {
          return yScale(d.virusName) + yScale.bandwidth() / 2;
        })
        .attr("x2", xScale(53536) - 14)
        .attr("y2", function(d) {
          return yScale(d.virusName) + yScale.bandwidth() / 2;
        })
        .attr("stroke", function(d) {
          if (
            xScale(d.directExport + d.indirectExport) + 14 <
            xScale(53536) - 14
          ) {
            return "white";
          } else {
            return "none";
          }
        })
        .attr("stroke-dasharray", "4")
        .attr("stroke-width", "1")
        .attr("transform", `translate(16,40)`);

      const yAxis = barChart
        .append("g")
        .attr("class", "axis axisY")
        .call(d3.axisLeft(yScale))
        .attr("transform", `translate(${containerMarginLeft},40)`)
        .style("color", "#a6a3a3")
        .style("font-size", "15px");

      const xAxis = barChart
        .append("g")
        .attr("class", "axis axisX")
        .call(
          d3
            .axisTop(xScale)
            .ticks(6, "s")
            .tickSizeOuter(0)
        )
        .attr("color", "#a6a3a3")
        .attr("transform", `translate(16,20)`)
        .attr("font-size", "15px");

      barChart
        .append("circle")
        .attr("r", "16")
        .attr("stroke", "#b60b0b")
        .attr("cx", "500")
        .attr("cy", "980")
        .attr("stroke-width", "3");

      barChart
        .append("circle")
        .attr("r", "16")
        .attr("stroke", "#a6a3a3")
        .attr("cx", "700")
        .attr("cy", "980")
        .attr("stroke-width", "3");

      barChart
        .append("line")
        .attr("x1", function(d) {
          return 500 + 14;
        })
        .attr("y1", function(d) {
          return 980;
        })
        .attr("x2", 700 - 14)
        .attr("y2", function(d) {
          return 980;
        })
        .attr("stroke", "white")
        .attr("stroke-dasharray", "4");

      barChart
        .append("text")
        .text("potential reservoirs trading amount")
        .attr("transform", "translate(470, 984)")
        .attr("text-anchor", "end")
        .attr("fill", "white");

      barChart
        .append("text")
        .text("total trading amount")
        .attr("transform", "translate(730, 984)")
        .attr("text-anchor", "start")
        .attr("fill", "white");

      d3.selectAll(".axis.axisY>.domain").remove();
      d3.selectAll(".tick line").remove();

      d3.selectAll("text").style("font-family", "Arial, Helvetica, sans-serif");

      d3.selectAll(".tick text")
        .call(wrap, 200)
        .attr("text-anchor", "end");

      function wrap(text, width) {
        text.each(function() {
          var text = d3.select(this),
            words = text
              .text()
              .split(/\s+/)
              .reverse(),
            word,
            line = [],
            lineNumber = 0,
            lineHeight = 1.1,
            x = text.attr("x"),
            y = text.attr("y"),
            dy = 0,
            tspan = text
              .text(null)
              .append("tspan")
              .attr("x", x)
              .attr("y", y)
              .attr("dy", dy + "em");
          while ((word = words.pop())) {
            line.push(word);
            tspan.text(line.join(" "));
            if (tspan.node().getComputedTextLength() > width) {
              line.pop();
              tspan.text(line.join(" "));
              line = [word];
              tspan = text
                .append("tspan")
                .attr("x", x)
                .attr("y", y)
                .attr("dy", ++lineNumber * lineHeight + dy + "em")
                .text(word);
            }
          }
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
  padding-top: 50px;
  padding-bottom: 50px;
}
</style>
