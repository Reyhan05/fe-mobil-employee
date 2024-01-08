<template>
    <div ref="chartRef"></div>
  </template>
  
  <script>
    import Highcharts from "highcharts";
  
    export default {
      props: {
        data: {
          type: Array,
          default: []
        },
        legendColor: {
          type: String,
          default: "",
        },
      },
      data() {
        return {
          chart: null,
          dataPie: null,
          data2: []
        };
      },
      methods: {
        initChart() {
          
          const chartOptions = {
            chart: {
              type: "pie",
              plotBackgroundColor: null,
              backgroundColor: "rgba(0, 0, 0, 0)",
              height: 300,
              plotBorderWidth: null,
              plotShadow: false,
            },
            plotOptions: {
              pie: {
                dataLabels: {
                  enabled: true,
                },
              },
            },
            title: {
              text: null,
            },
            legend: {
              layout: "horizontal",
              align: "center",
              verticalAlign: "bottom",
              itemStyle: {
                width: 190,
                color: this.legendColor,
              },
              floating: false,
              borderWidth: 0,
              shadow: false,
  
              formatter: function () {
                //use formatter to break word.
                return (
                  '<div align="center" style="word-wrap: break-word;word-break: break-all;width:50px">' +
                  this.value +
                  "</div>"
                );
              },
            },
            tooltip: {
              valueSuffix: "",
              enabled: false,
            },
            accessibility: {
              point: {
                valueSuffix: "%",
              },
            },
            plotOptions: {
              pie: {
                allowPointSelect: true,
                cursor: "pointer",
                dataLabels: {
                  enabled: false,
                },
                showInLegend: true,
              },
            },
            series: [{
              data: this.data2
            }],
            responsive: {
              rules: [{
                condition: {
                  maxWidth: 500,
                },
                chartOptions: {
                  legend: {
                    layout: "horizontal",
                    align: "center",
                    verticalAlign: "bottom",
                  },
                },
              }, ],
            },
            series: [{
              type: "pie",
              data: this.data2
            }, ],
          };
          
          this.chart = Highcharts.chart(this.$refs.chartRef, chartOptions);
        },
      },
      watch: {},
      mounted() {
        this.data.forEach((element) => {
          this.data2.push({
            name: element.nama,
            y: element.umur
          })
        })
        this.initChart();
          console.log(this.data)
          console.log(this.data2)
        // this.asyncData();
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

  </style>