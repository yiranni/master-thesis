<template>
  <div class="main">
    <!-- <ul>
      <li v-for="g in tradedGenusWithVirus">Animal Name: {{g.genusName}}, Amount: {{g.amount}}</li>
    </ul>-->
    <div id="bubbleChart">
      <svg ref="quantityBubbles" id="qantityBubbles" />
    </div>
  </div>
</template>

<script>
import allTrade2019 from "~/assets/data/allTrade2019.json";
import VirusData from "~/assets/virus-grouped.json";
import TradingQuantity from "~/assets/data/allGenusQuantity.json";

import animalName from "~/assets/animalName.json";
import * as d3 from "d3";
export default {
  name: "TradeWithVirus",
  props: ["selectedVirus"],
  data() {
    return {
      animalName: animalName,
      allTrade: allTrade2019,
      TradingQuantity: TradingQuantity,
      VirusData: VirusData,
      updatedVirusName: null,
      updatedVirusData: null,
      allGenusWithUpdatedVirus: null,
      tradedGenusWithVirus: null,
      width: 700,
      height: 300
    };
  },
  mounted() {
    this.updateData();
    this.updateSelectedVirusData();
    this.parseData();
    this.findGenusTraded();
    this.createBubble();
  },
  methods: {
    updateData() {
      this.updatedVirusName = this.$props.selectedVirus;
    },
    updateSelectedVirusData() {
      this.updatedVirusData = VirusData[this.updatedVirusName];
    },
    parseData() {
      let allGenus = [];
      let selectedVirusData = this.updatedVirusData;
      for (let family in selectedVirusData) {
        selectedVirusData[family].genus.forEach(e => {
          allGenus.push(e.genusName);
        });
      }
      this.allGenusWithUpdatedVirus = allGenus;
    },
    findGenusTraded() {
      let allTraded = this.allTrade;
      let allGenusWithThisVirus = this.allGenusWithUpdatedVirus;
      // console.log(allTraded)
      let tradedWithVirus = [];
      allTraded.forEach(e => {
        let tradedGenus = e.genusName;
        let tradeAmount =
          e /
          allGenusWithThisVirus.forEach(g => {
            if (tradedGenus === g) {
              let obj = {};

              obj.genusName = tradedGenus;
              let tradeDetails = this.TradingQuantity.find(
                e => e.genus_name === tradedGenus
              );
              obj.amount = tradeDetails.Exporter_reported_quantity;
              if (obj.amount > 0) {
                tradedWithVirus.push(obj);
              }
              console.log(tradedWithVirus);
            }
          });
      });
      this.tradedGenusWithVirus = tradedWithVirus;
    },
    createBubble() {
      const data = this.tradedGenusWithVirus;
      const animalData = this.animalName;
      const scale = d3.scaleLinear().range([20, 60]);

      const div = d3
        .select("#bubbleChart")
        .append("div")
        .attr("class", "tooltip")
        .attr("width", "500")
        .style("opacity", 0);

      scale.domain([
        0,
        d3.max(data, function(d) {
          return d.amount;
        })
      ]);

      const bubbleContainer = this.$refs.quantityBubbles;
      let containerWidth = this.width;
      let containerHeight = this.height;

      const simulation = d3
        .forceSimulation()
        .force("x", d3.forceX(containerWidth / 2).strength(0.2))
        .force("y", d3.forceY(containerHeight / 2).strength(0.2))
        .force(
          "collide",
          d3.forceCollide(function(d) {
            return scale(d.amount) + 10;
          })
        );
      //   const first = scale(data[0].amount)

      const bubbleChart = d3
        .select(bubbleContainer)
        .attr("width", containerWidth)
        .attr("height", containerHeight)

        .attr("transform", `translate(0,0)`);

      const bubbles = bubbleChart
        .selectAll(".bubble")
        .data(data)
        .enter()
        .append("g");

      const circles = bubbles
        .append("circle")
        .attr("r", function(d) {
          return scale(d.amount);
        })
        .attr("class", "tradeQuant")
        .style("fill", "#FF6496")
        .on("mouseover", function(d) {
          d3.select(this)
            .attr("stroke", "white")
            .attr("stroke-width", "2");
        let matchedGroup = animalData.find(e => e.genusName === d.genusName);
        let commonName = matchedGroup.commonName
        console.log(matchedGroup)
          div
            .html(
              `<h1 style="font-size: 20px; font-weight: normal; color: #f73179">${commonName}</h1><h2 style="color: #929292; font-style: oblique; font-weight: normal; font-size: 16px; padding-top: 10px; padding-bottom: 10px">Genus: ${d.genusName} </h1> <hr style="border-top: 1px solid #f73179"> <p style="padding-top: 16px">Traded: ${d.amount}</p>`
            )
            .style("position", "absolute")
            .style("background", "black")
            .style("border", "2px #f73179 solid")
            // .style("border-color", "white")
            .style("width", "200px")
            // .style("height", "80px")
            .style("padding", "12px")
            .style("opacity", 1)
            .style("left", d3.event.pageX + 20 + "px")
            .style("top", d3.event.pageY + "px");
        })
        .on("mouseout", function(d) {
          d3.select(this).attr("stroke", "none");
          div.style("opacity", 0);
        });

      //         position: absolute;
      //   text-align: center;
      //   left: 200;
      //   top: 0;
      //   width: 60px;
      //   height: 28px;
      //   padding: 2px;
      //   font: 12px sans-serif;
      //   background: lightsteelblue;
      //   border: 0px;
      //   border-radius: 8px;
      //   pointer-events: none;

      simulation.nodes(data).on("tick", ticked);

      function ticked() {
        circles
          .attr("cx", function(d) {
            return d.x;
          })
          .attr("cy", function(d) {
            return d.y;
          });
      }

      function handleMouseOver(d, i) {
        // d3.select(this)
        //   .attr("stroke", "white")
        //   .attr("stroke-width", "2");
        // tooltip
        //   .transition()
        //   .duration(200)
        //   .style("opacity", 0.9);
        // tooltip
        //   .html("tooltip")
        // //   .style("display", "inline")
        //   .style("left", d3.select(this).attr("cx") + "px")
        //   .style("top", d3.select(this).attr("cy") + "px");
      }

      function handleMouseOut(d, i) {
        d3.select(this).attr("stroke", "none");
        // tooltip.duration(500).style("opacity", 0);
      }
    }
  },
  watch: {
    selectedVirus() {
      this.updateData();
      this.updateSelectedVirusData();
      this.parseData();
      this.findGenusTraded();
      this.createBubble();
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
