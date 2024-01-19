<template>
  <div>
    <div ref="chartRef" style="position: relative;"></div>
    <div class="custom-label">90.2%</div>
    <div class="labels">
      <p class="text-light label" style="padding: .4rem .8rem ;background: #06D391; font-weight: 600;">+10.2%</p>
    </div>
  </div>
</template>

<script>
import Highcharts from "highcharts";
import HighchartsMore from "highcharts/highcharts-more";
import SolidGauge from "highcharts/modules/solid-gauge";
import HighchartsVue from "highcharts-vue";

HighchartsMore(Highcharts);
SolidGauge(Highcharts);

export default {
  // components: {
  //   HighchartsVue,
  // },
  data() {
    return {  
      chart: null,
    };
  },
  methods: {
    initChart() {
      const chartOptions = {
        chart: {
          type: "solidgauge",
          height: 300,
          backgroundColor: "rgba(0, 0, 0, 0)",
        },
        title: null,
        pane: {
          center: ["50%", "85%"],
          size: "130%",
          startAngle: -90,
          endAngle: 90,
          background: {
            backgroundColor:
              Highcharts.defaultOptions.legend.backgroundColor || "#EEE",
            innerRadius: "60%",
            outerRadius: "100%",
            shape: "arc",
          },
        },
        tooltip: {
          enabled: true,
        },
        yAxis: {
          stops: [
            [0.1, "#DF5353"], // red
            [0.5, "#DDDF0D"], // yellow
            [0.9, "#55BF3B"], // green
          ],
          lineWidth: 0,
          minorTickInterval: null,
          tickPixelInterval: 400,
          tickWidth: 0,
          title: {
            y: -70,
          },
          labels: {
            y: 16,
          },
        },
        plotOptions: {
          solidgauge: {
            dataLabels: {
              y: 5,
              borderWidth: 0,
              useHTML: true,
            },
          },
        },
        legend: {
          enabled: false,
        },
        series: [
          {
            name: "Speed",
            data: [80],
            dataLabels: {
              format:
                '<div style="text-align:center"><span style="font-size:25px;color:' +
                (Highcharts.theme && Highcharts.theme.contrastTextColor) ||
                "black" +
                '">{y}%</span><br/>' +
                '<span style="font-size:12px;color:silver">km/h</span></div>',
            },
          },
          {
            name: "Left Label",
            data: [75],
            dataLabels: {
              format: '<div style="text-align:center"><span style="font-size:12px;color:black">{y}%</span></div>',
              x: -150,
              y: 9,
            },
          },
          {
            name: "Right Label",
            data: [85],
            dataLabels: {
              format: '<div style="text-align:center"><span style="font-size:12px;color:black">{y}%</span></div>',
              x: 150,
              y: 9,
            },
          },
        ],
      };

      this.chart = Highcharts.chart(this.$refs.chartRef, chartOptions);
    },
  },
  mounted() {
    this.initChart();
  },
};
</script>

<style>
.highcharts-credits {
  font-size: 0px !important;
}

#highcharts-jk2apwh-44 {
  width: 100% !important;
}

.custom-label {
  position: absolute;
  top: 50%;
  left: 50%;
  margin-top: 35px;
  transform: translate(-50%, -50%);
  font-size: 30px;
  color: black;
}

.label {
  position: absolute;
  top: 50%;
  left: 50%;
  margin-top: 15%;
  transform: translate(-50%, -50%);
  font-size: 20px;
  border-radius: 10px;
}
</style>