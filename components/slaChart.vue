<template>
    <!-- <highcharts :options="dataChart" v-if="!loading"></highcharts> -->
    <div ref="chartRef" class="mx-2"></div>
  </template>
  
  <script>
  import { Chart } from "highcharts-vue";
  import More from "highcharts/highcharts-more";
  import Highcharts from "highcharts";
  More(Highcharts);
  let timer = null;
  export default {
    components: {
      highcharts: Chart,
    },
    props: {
      loading: {
        type: Boolean,
        default: false,
      },
      height: {
        type: Number,
        default: 150,
      },
      itemH: {
        type: Number,
        default: null
      },
      data: {
        type: Object,
        default: {},
      },
      size: {
        type: String,
        default: null,
      },
    },
    data() {
      return {
        chart: null,
        dataSLA: [],
      };
    },
    methods: {
    //   getdata() {
    //     try {
    //       this.dataSLA[0] = parseFloat(this.data.totalSLA.toFixed(2));
    //       // var uptime =
    //       //   (this.data.totalSLA.toFixed(2) / 100) *
    //       //   parseInt(this.data.minutesMonth);
    //       // this.timeconvertMinutes
    //       // this.convertMinutes(this.data.minutesMonth - uptime);
    //     } catch (error) {
    //       this.dataSLA[0] = 0;
    //     }
    //     // console.log(this.dataSLA)
    //   }
    },
    mounted() {
    //   this.getdata();
      let divider = 95;
      if (this.dataSLA[1] != "" && this.dataSLA[1] != undefined) {
        divider = this.dataSLA[1];
        // divider = 88;
        console.log(divider);
      }
      const dataChart = {
        chart: {
          type: "gauge",
          plotBackgroundColor: null,
          plotBackgroundImage: null,
          backgroundColor: "rgba(0, 0, 0, 0)",
          plotBorderWidth: 0,
          plotShadow: false,
          // height: this.size == 'lg' ? 450 : this.height,
          height: this.itemH * 26,
          // height: "80%",
          marginLeft:40,
          marginRight:40
        },
  
        title: {
          text: " ",
        },
  
        pane: {
          startAngle: -90,
          endAngle: 89.9,
          background: null,
          center: ["50%", "75%"],
          size: "110%",
        },
  
        // the value axis
        yAxis: {
          min: this.dataSLA[0] < 80 ? this.dataSLA[0] : 80,
          max: 100,
          tickPixelInterval: 50,
          tickPosition: "inside",
          tickColor: Highcharts.defaultOptions.chart.backgroundColor || "#FFFFFF",
          tickLength: 20,
          tickWidth: 2,
          minorTickInterval: null,
          labels: {
            distance: 10,
            style: {
              fontSize: "14px",
            },
          },
          plotBands: [
            {
              from: divider,
              to: 100,
              color: "#55BF3B", // green
              thickness: 20,
            },
            {
              from: 0,
              to: divider,
              color: "#DF5353", // red
              thickness: 20,
            },
          ],
        },
  
        series: [
          {
            name: "Uptime",
            data: this.dataSLA,
            tooltip: {
              valueSuffix: " %",
            },
            dataLabels: {
              format: "{y} %",
              borderWidth: 0,
              color:
                (Highcharts.defaultOptions.title &&
                  Highcharts.defaultOptions.title.style &&
                  Highcharts.defaultOptions.title.style.color) ||
                "#333333",
              style: {
                fontSize: "16px",
              },
            },
            dial: {
              radius: "80%",
              backgroundColor: "gray",
              baseWidth: 12,
              baseLength: "0%",
              rearLength: "0%",
            },
            pivot: {
              backgroundColor: "gray",
              radius: 6,
            },
          },
        ],
      };
  
      this.chart = Highcharts.chart(this.$refs.chartRef, dataChart);
    },
  };
  </script>
  
  <style>
  .highcharts-figure,
  .highcharts-data-table table {
    min-width: 220px;
    max-width: 800px;
    margin: 1em auto;
  }
  
  .highcharts-data-table table {
    font-family: Verdana, sans-serif;
    border-collapse: collapse;
    border: 1px solid #ebebeb;
    margin: 10px auto;
    text-align: center;
    width: 100%;
    max-width: 500px;
  }
  
  .highcharts-data-table caption {
    padding: 1em 0;
    font-size: 1.2em;
    color: #555;
  }
  
  .highcharts-data-table th {
    font-weight: 600;
    padding: 0.5em;
  }
  
  .highcharts-data-table td,
  .highcharts-data-table th,
  .highcharts-data-table caption {
    padding: 0.5em;
  }
  
  .highcharts-data-table thead tr,
  .highcharts-data-table tr:nth-child(even) {
    background: #f8f8f8;
  }
  
  .highcharts-data-table tr:hover {
    background: #f1f7ff;
  }
  
  .ld-label {
    width: 200px;
    display: inline-block;
  }
  
  .ld-url-input {
    width: 500px;
  }
  
  .ld-time-input {
    width: 40px;
  }
  
  .highcharts-credits {
    font-size: 0px !important;
  }
  </style>  