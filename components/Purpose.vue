<template>
  <div>
    <svg ref="ring" />
  </div>
</template>

<script>
import * as d3 from "d3";
import TradeData from "~/assets/data/trade/Acinonyx.json";

export default {
  name: "Purpose",
  components: {},
  props: ["trackAnimal", "dataPath"],
  data() {
    return {
      selectedDataPath: "",
      animalData: [],
      width: 340,
      height: 340,
      colorOffset: 0.4
    };
  },
  //   async asyncData() {
  //     const { data } = await axios.get(this.$props.dataPath);
  //     return { selectedDataPath: data };
  //   },
  mounted() {
    this.getDataPath();
    // this.importData();
    this.createRingDiagram();
  },

  methods: {
    getDataPath() {
      let genusName = this.$props.trackAnimal;
      this.selectedDataPath = this.$props.dataPath;
    },
    // importData() {
    //     console.log(this.dataPath);
    //     let data = import(this.dataPath);
    //     .then(m => {
    //       console.log(m.Year);
    //     })
    //     .catch(err => {
    //       console.error("Error during loading module: " + err);
    //     //   callback();
    //     });
    //   console.log(data);
    //   console.log(data)
    // let promise = new Promise((res, rej) => {
    //   res(data);
    // });
    //   let selectedData = [];
    // promise.then(val => {
    //   for (let v of Object.entries(val)) {
    //     if (v[0] != "default") {
    //       selectedData.push(v[1]);
    //     }
    //   }
    // });
    //   console.log(selectedData)
    //   this.animalData = selectedData;
    // },

    createRingDiagram() {
      // const data = this.animalData;
      // Array.prototype.forEach.call(this.animalData, d => {
      //   console.log(d);
      // });
      // console.log(data)
      const data = TradeData;
      const nestedPurpose = d3
        .nest()
        .key(d => d.Term)
        .rollup(d =>
          d3.sum(d, function(d) {
            return d.Exporterreportedquantity;
          })
        )
        .entries(data);

      console.log(nestedPurpose);

      const svgDom = this.$refs.ring;

      svgDom.innerHTML = ``;

      const svg = d3
        .select(svgDom)
        // .attr("viewBox", [
        //   -this.width / 2,
        //   -this.height / 2 - 20,
        //   this.width,
        //   this.height + 40
        // ])

        .attr("width", this.width)
        .attr("height", this.height);

      const radius = this.width / 2 - 20;

      const arc = d3
        .arc()
        .innerRadius(radius * 0.8)
        .outerRadius(radius - 1);

      const color = d3
        .scaleOrdinal()
        .domain(
          nestedPurpose.map(function(d) {
            return d.key;
          })
        )
        .range(
          d3
            .quantize(
              t => d3.interpolateRdPu(t * this.colorOffset + 0.5),
              nestedPurpose.length
            )
            .reverse()
        );

      const pie = d3
        .pie()
        .sort(null)
        .value(function(d) {
          return d.value;
        });

      const arcs = pie(nestedPurpose);

      const donoutChart = svg
        .selectAll(".arc")
        .data(arcs)
        .enter()
        .append("g")
        .attr("class", "pie")
        .attr("transform", `translate(${this.width / 2}, ${this.height / 2})`);

      donoutChart
        .append("path")
        .attr("fill", d => color(d.data.key))
        .transition()
        .delay(function(d, i) {
          return i * 500;
        })
        .attrTween("d", function(d) {
          var i = d3.interpolate(d.startAngle, d.endAngle);
          return function(t) {
            d.endAngle = i(t);
            return arc(d);
          };
        });

      

      let total = 0;
      nestedPurpose.forEach(e => {
        total += e.value;
      });

      const count = svg
        .append("text")
        .text(total)
        .attr("fill", "white")
        .attr("text-anchor", "middle")
        .attr("font-size", "24")
        .attr("class", "count")
        .attr("x", this.width / 2)
        .attr("y", this.height / 2 - 20);

      const defaultTrade = svg
        .append("text")
        .text("total traded")
        .attr("fill", "white")
        .attr("text-anchor", "middle")
        .attr("font-size", "18")
        .attr("x", this.width / 2)
        .attr("y", this.height / 2 + 10)
        .attr("class", "defaultTrade");
      // console.log(total)

      let diagramWidth = this.width;
      let diagramHeight = this.height;

      const type = svg
        .append("text")
        .text("")
        .attr("fill", "white")
        .attr("text-anchor", "middle")
        .attr("font-size", "24")
        .attr("x", diagramWidth / 2)
        .attr("y", diagramHeight / 2 - 30)
        .attr("class", "type");

      const percentage = svg
        .append("text")
        .text("")
        .attr("fill", "white")
        .attr("text-anchor", "middle")
        .attr("font-size", "18")
        .attr("x", diagramWidth / 2)
        .attr("y", diagramHeight / 2 + 30)
        .attr("class", "percentage");

      svg.selectAll(".pie").on("mouseover", handleMouseOver);
      svg.selectAll(".pie").on("mouseout", handleMouseOut);

      function handleMouseOver(d, i) {
        d3.select(".count")
          .text(d.data.value + " traded")
          .attr("font-size", "18")
          .attr("x", diagramWidth / 2)
          .attr("y", diagramHeight / 2);

        d3.select(".defaultTrade").text("");

        d3.select(".percentage").text(
          Math.round((d.data.value / total) * 100, 0) + "%"
        );

        d3.select(".type").text(camelize(d.data.key));
        function camelize(str) {
          return str
            .split(" ")

            .map(a => a.trim())

            .map(a => a[0].toUpperCase() + a.substring(1))
            .join("");
        }
      }

      function handleMouseOut(d, i) {
        d3.select(".percentage").text("");
        d3.select(".type").text("");
        d3.select(".count")
          .text(total)
          .attr("fill", "white")
          .attr("text-anchor", "middle")
          .attr("font-size", "24")
          .attr("class", "count")
          .attr("x", diagramWidth / 2)
          .attr("y", diagramHeight / 2 - 20);

        d3.select(".defaultTrade").text("total traded");
      }
    }
  },
  watch: {
    dataPath() {
      this.getDataPath();
      // this.importData();
      //   this.createRingDiagram();
    }

    // colorOffset() {
    //   this.createRingDiagram();
    // }
  }
};
</script>

<style scoped>
p {
  color: white;
  font-size: 16px;
}

li {
  color: white;
}

svg {
  margin-top: 40px;
}
</style>
