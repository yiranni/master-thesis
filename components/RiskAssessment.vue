<template>
  <div class="main">
    <svg :viewBox="[0, 0, width, height]" :width="width" :height="height" ref="risk">
      <g
        v-for="(animal, i) in riskTrade"
        :id="animal.genusName"
        :key="i"
        :transform="`translate(${i % 3 * 300}, ${Math.floor(i/3) * 300})`"
      >
        <!-- <rect :width="250" :height="440" stroke="white"/> -->
        <line :x1="0" :y1="220" :x2="220" :y2="220" stroke="white" />
        <rect
          v-for="count in Math.ceil(animal.importAmount / 1000) "
          :x="((count - 1)% 8) * 25 + 10"
          :y="220+ 25* Math.floor((count-1)/8)+15"
          :width="20"
          :height="15"
          fill="#b60b0b"
        />

        <rect
          v-for="count in Math.ceil(animal.exportAmount / 1000) "
          :x="((count - 1)% 8) * 25 + 10"
          :y="220- 25* Math.floor((count-1)/8)-30"
          :width="20"
          :height="15"
          fill="#a6a3a3"
        />

        <text :x="110" :y="280" fill="white" text-anchor="middle" font-weight="bold">{{animal.commonName}}</text>
        <text :x="110" :y="310" fill="white" text-anchor="middle">Genus: {{animal.genusName}}</text>
        <text :x="110" :y="330" fill="white" text-anchor="middle">Risk Score: {{animal.score}}</text>
      </g>
      <rect :width="10" :height="10" :x="200" :y="50" fill="white" />
      <text :x="220" :y="62" fill="white">= 1000 species</text>

      <rect :width="10" :height="10" :x="360" :y="50" fill="#a6a3a3" />
      <text :x="380" :y="62" fill="white">Imported</text>

      <rect :width="10" :height="10" :x="480" :y="50" fill="#b60b0b" />
      <text :x="500" :y="62" fill="white">Exported</text>
    </svg>
  </div>
</template>

<script>
import reservoirs from "~/assets/data/reservoirs.json";
import allTrade from "~/assets/data/allTrade.json";
import * as d3 from "d3";
export default {
  name: "RiskAssessment",
  props: [],
  data() {
    return {
      reservoirs: reservoirs,
      allTrade: allTrade,
      riskData: null,
      genusGrouped: null,
      riskTrade: null,
      width: 820,
      height: 700,
      top6Risk: null
    };
  },
  mounted() {
    this.calculateRisk();
    this.getGenusGroupedData();
    this.getTopRisk();
    this.getRiskTrade();
    this.createLineChart();
  },
  methods: {
    calculateRisk() {
      let data = reservoirs;

      let riskData = [];

      reservoirs.forEach(r => {
        let obj = {};

        let objValues = Object.values(r);
        let score = 0;

        for (let i = 3; i < objValues.length; i++) {
          score += parseInt(objValues[i]);
        }

        obj.genusName = r.genusName;
        obj.commonName = r.commonName;
        obj.score = score;
        riskData.push(obj);
      });

      this.riskData = riskData;
    },
    getGenusGroupedData() {
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
    getTopRisk() {
      const data = this.riskData;

      const genusData = this.genusGrouped;
      let sortedData = data.sort(function(a, b) {
        return b.score - a.score;
      });

      let top6Risky = [];

      for (let i = 0; i < 6; i++) {
        top6Risky.push(sortedData[i]);
      }

      this.top6Risk = top6Risky;
    },
    getRiskTrade() {
      let riskData = this.top6Risk;
      let tradeData = this.genusGrouped;

      let riskTrade = [];

      for (let i = 0; i < riskData.length; i++) {
        let obj = {};
        let genusName = riskData[i].genusName;
        let commonName = riskData[i].commonName;
        let score = riskData[i].score;
        obj.genusName = genusName;
        obj.commonName = commonName;
        obj.score = score;
        let found = tradeData.find(e => e[0] === genusName);
        let trade = { ...found[1] };
        let exportAmount = trade.Exporterreportedquantity;
        let importAmount = trade.Importerreportedquantity;
        obj.importAmount = importAmount;
        obj.exportAmount = exportAmount;
        riskTrade.push(obj);
        // console.log(found[1])
      }
      this.riskTrade = riskTrade;
      // console.log(riskTrade)
    },
    createLineChart() {
      //   let height = this.height;
      //   let width = this.width;
      //   const data = this.riskTrade;
      //   const boxChartContainer = this.$refs.risk;
      //   let containerMarginTop = 20;
      //   let containerMarginBottom = 40;
      //   let containerMarginRight = 0;
      //   let containerMarginLeft = 50;
      //   let containerWidth = width - containerMarginLeft - containerMarginRight;
      //   let containerHeight = height - containerMarginTop - containerMarginBottom;
      //   //   const nestedData = d3
      //   //     .nest()
      //   //     .key(function(d) {
      //   //       return d.Dateofjoiningsortdescending;
      //   //     })
      //   //     .entries(data);
      //   //   let cumulatedByDate = [];
      //   //   let runningAmount = 0;
      //   //   for (let i = 0; i < nestedData.length; i++) {
      //   //     let obj = {};
      //   //     obj.date = nestedData[i].key;
      //   //     runningAmount = runningAmount + nestedData[i].values.length;
      //   //     obj.runningAmount = runningAmount;
      //   //     cumulatedByDate.push(obj);
      //   //   }
      //   const chart = d3
      //     .select(boxChartContainer)
      //     .attr(
      //       "width",
      //       containerWidth + containerMarginLeft + containerMarginLeft
      //     )
      //     .attr(
      //       "height",
      //       containerHeight + containerMarginTop + containerMarginBottom
      //     )
      //     .append("g");
      //   const animalsContainer = chart
      //     .selectAll(".animal")
      //     .data(data)
      //     .enter()
      //     .append("g");
      //   animalsContainer
      //     .append("line")
      //     .attr("x1", function(d, i) {
      //         console.log(i)
      //       return (i % 3) * 300;
      //     })
      //     .attr("y1", function(d, i) {
      //       return Math.floor(i / 3) * 270 + 220;
      //     })
      //     .attr("x2", function(d, i) {
      //        return  (i % 3) * 300 + 250
      //     })
      //     .attr("y2", function(d, i) {
      //          return Math.floor(i / 3) * 270 + 220;
      //     })
      //     .attr("stroke", "white")
      //   let parseTime = d3.timeParse("%d/%m/%Y");
      //   cumulatedByDate.forEach(function(d) {
      //     d.date = parseTime(d.date);
      //     d.runningAmount = d.runningAmount;
      //   });
      //   const x = d3.scaleTime().range([0, containerWidth]);
      //   const y = d3.scaleLinear().range([containerHeight, 0]);
      //   x.domain(
      //     d3.extent(cumulatedByDate, function(d) {
      //       return d.date;
      //     })
      //   );
      //   y.domain([
      //     0,
      //     d3.max(cumulatedByDate, function(d) {
      //       return d.runningAmount;
      //     })
      //   ]);
      //   lineChart
      //     .append("g")
      //     .attr("class", "xAxis")
      //     .attr("transform", `translate(0, ${containerHeight})`)
      //     .style("color", "#A6A3A3")
      //     .style("font-size", "18px")
      //     .call(d3.axisBottom(x).tickSize(0).tickPadding(20).ticks(6));
      //   lineChart
      //     .append("g")
      //     .attr("class", "yAxis")
      //     .style("color", "#A6A3A3")
      //     .style("font-size", "18px")
      //     .call(d3.axisLeft(y).tickSize(0).tickPadding(20).ticks(5));
      //   const valueLine = d3
      //     .line()
      //     .x(function(d) {
      //       return x(d.date);
      //     })
      //     .y(function(d) {
      //       return y(d.runningAmount);
      //     });
      //   lineChart
      //     .append("path")
      //     .data([cumulatedByDate])
      //     .attr("class", "line")
      //     .attr("d", valueLine)
      //     .attr("fill", "none")
      //     .attr("stroke-width", "3")
      //     .attr("stroke", "#b60b0b");
      //   d3.selectAll(".yAxis>.tick>line").remove()
      //   d3.selectAll(".xAxis>.tick>line").remove()
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
R