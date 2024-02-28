<template>
  <div>
    <div ref="charts" style="width: 400px; height: 200px"></div>
  </div>
</template>

<script>
import * as echarts from "echarts";
export default {
  name: "XianChar",
  props: {
    temperatureData: {
      type: Array,
    },
    humidityData: {
      type: Array,
    },
  },
  data() {
    return {};
  },
  methods: {
    getChart() {
      this.chart = echarts.init(this.$refs.charts);
      this.chart.setOption({
        // title: {
        //   text: "温度",
        // },
        tooltip: {
          trigger: "axis",
        },
        xAxis: {
          type: "time",
          splitLine: {
            show: false,
          },
        },
        yAxis: {
          type: "value",
        },
        legend: {
          data: ["temperature", "humidity"],
        },
        series: [
          {
            name: "temperature",
            data: this.temperatureData,
            type: "line",
            showSymbol: false,
            hoverAnimation: false,
            lineStyle: {
              width: 1,
              color: "#5470c6",
            },
            smooth: true,
            // areaStyle: {},
          },
          {
            name: "humidity",
            data: this.humidityData,
            type: "line",
            showSymbol: false,
            hoverAnimation: false,
            lineStyle: {
              width: 1,
              color: "#91cc75",
            },
            smooth: true,
            // areaStyle: {},
          },
        ],
      });
    },
  },
  mounted() {
    this.$nextTick(() => {
      this.getChart();
    });
  },
  beforeUnmount() {
    if (this.chart) {
      this.chart.dispose();
    }
  },
};
</script>

<style>
* {
  padding: 0%;
  margin: 0%;
}
</style>
