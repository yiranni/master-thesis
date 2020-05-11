<template>
  <div class="main">
    <div id="reservoirBubbleChart">
      <svg ref="reservoirBubbles" id="reservoirBubbles" />
    </div>
  </div>
</template>

<script>
import allTrade2019 from "~/assets/data/allTrade2019.json";
import VirusData from "~/assets/virus-grouped.json";

import animalName from "~/assets/animalName.json";
import * as d3 from "d3";
export default {
  name: "AllReservoirs",
  props: [],
  data() {
    return {
      animalName: animalName,
      allTrade: allTrade2019,
      //   TradingQuantity: TradingQuantity,
      VirusData: VirusData,
      VirusArray: null,
      allGenera: null,
      width: 1000,
      height: 1300
    };
  },
  mounted() {
    this.parseData();
    this.getAllGenera();
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
    getAllGenera() {
      let res = [];
      let data = this.VirusArray;
      data.forEach(e => {
        let virusName = e.virusName;
        let reservoirs = e.reservoirs;
        reservoirs.forEach(r => {
          let obj = {};
          obj.virusName = virusName;
          obj.genusName = r.genusName;
          obj.commonName = r.commonName;
          obj.direct = r.direct;
          res.push(obj);
        });
      });

      this.allGenera = res;
    },
    createBubble() {
      const data = this.allGenera;
      const nested = d3
        .nest()
        .key(function(d) {
          return d.virusName;
        })
        .entries(data);

      console.log(nested);

      const virusNameArray = [
        "Rift Valley Fever Virus",
        "Ebola Virus",
        "Crimean Congo Hemorrhagic Fever Virus",
        "Tick-Borne Encephalitis Virus",
        "Venezuelan Equine Encephalitis Virus",
        "Rabies Virus",
        "SARS",
        "H5N1",
        "Nipah Virus",
        "Yellow Fever Virus",
        "Lymphocytic Choriomeningitis Virus",
        "Cercopithecine Herpes Virus",
        "Marburg Virus",
        "HCPS",
        "HFRS",
        "South American Hemorrhagic Fever Arenaviruses",
        "Lassa Fever Virus"
      ];
      const radius = 10;
      const bubbleContainer = this.$refs.reservoirBubbles;
      let containerWidth = this.width;
      let containerHeight = this.height;

      const forceX = d3
        .forceX(function(d) {
          let matched = virusNameArray.find(e => e === d.virusName);
          let i = virusNameArray.indexOf(matched);
          return (i % 4) * 230 + 150;
        })
        .strength(1);

      const forceY = d3
        .forceY(function(d) {
          let matched = virusNameArray.find(e => e === d.virusName);
          let i = virusNameArray.indexOf(matched);
          return Math.floor(i / 4) * 250 + 100;
        })
        .strength(1);
      const simulation = d3
        .forceSimulation()
        .force("x", forceX)
        .force("y", forceY)
        .force(
          "collide",
          d3.forceCollide(function(d) {
            return radius + 4;
          })
        );
      //   const first = scale(data[0].amount)
      const div = d3
        .select("#reservoirBubbleChart")
        .append("div")
        .attr("class", "tooltip")
        .attr("width", "200")
        .style("opacity", 0);

      const bubbleChart = d3
        .select(bubbleContainer)
        .attr("width", containerWidth)
        .attr("height", containerHeight)
        .attr("transform", `translate(0,0)`);

      const bubbles = bubbleChart
        .selectAll(".reservoirsBubble")
        .data(data)
        .enter()
        .append("g");

      const circles = bubbles
        .append("circle")
        .attr("r", radius)
        .attr("fill", function(d) {
          if (d.direct === true) {
            return "#b60b0b";
          } else {
            return "#A6A3A3";
          }
        })
        .on("mouseover", function(d) {
          d3.select(this)
            .attr("stroke", "white")
            .attr("stroke-width", "2");
          div
            .html(
              `<h1 style="font-size: 16px; font-weight: bold; color: #b60b0b">${d.commonName}</h1><p style="color: #A6A3A3; font-size: 16px; font-style: oblique">Genus: ${d.genusName}</p>`
            )
            .style("position", "absolute")
            .style("background", "black")
            .style("border", "2px #b60b0b solid")
            .style("padding", "12px")
            .style("opacity", 1)
            .style("left", d3.event.pageX + 20 + "px")
            .style("top", d3.event.pageY + "px");
        })
        .on("mouseout", function(d) {
          d3.select(this).attr("stroke", "none");
          div.style("opacity", 0);
        });

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
      const labels = bubbleChart
        .selectAll(".virusLabels")
        .data(nested)
        .enter()
        .append("g");

      const virusName = labels
        .append("text")
        .text(function(d) {
          return d.key;
        })
        .attr("x", function(d, i) {
          return (i % 4) * 230 + 150;
        })
        .attr("y", function(d, i) {
          return Math.floor(i / 4) * 250 + 220;
        })
        .attr("fill", "white")
        .attr("text-anchor", "middle")
        .call(wrap, 180);

      bubbleChart
        .append("circle")
        .attr("r", "5")
        .attr("fill", "#b60b0b")
        .attr("transform", `translate(930, 8)`);

      bubbleChart
        .append("text")
        .text("direct")
        .attr("fill", "white")
        .attr("transform", `translate(945, 12)`);
      bubbleChart
        .append("circle")
        .attr("r", "5")
        .attr("fill", "#A6A3A3")
        .attr("transform", `translate(930, 30)`);
      bubbleChart
        .append("text")
        .text("indirect")
        .attr("fill", "white")
        .attr("transform", `translate(945, 36)`);
      bubbleChart;

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
  padding: 0;
  margin: 0;
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
