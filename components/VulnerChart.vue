<template>
    <div class="chart-container" :class="{'darkmode': darkmode}">
      <div ref="chartRef" style="float: left;"></div>
    </div>
  </template>
  
  <script>
    import Highcharts from 'highcharts';
    export default {
      props: {
        itemW: {
          type: Number,
          default: null
        },
        itemH: {
          type: Number,
          default: null
        },
      },
      data() {
        return {
          chart: null,
        };
      },
      methods: {
        initChart() {
          const konfigurasiLegend = this.itemW < 5 ? {
            subtitle: {
            useHTML: true,
            text: '<span class="number" style="font-size: 30px"><b>20</b></span><br><span class="vulner-text">Vulnerabilities</span>',
            floating: false,
            verticalAlign: 'middle',
            y: -2,
            x: 0, // Adjust the horizontal position as needed
          }
          } : { 
            subtitle: {
              useHTML: true,
              text: '<span class="number" style="font-size: 30px"><b>20</b></span><br><span class="vulner-text">Vulnerabilities</span>',
              floating: false,
              verticalAlign: 'middle',
              y: -5,
              x: -0
            },
          };
          const chartOptions = {
            chart: {
              type: 'pie',
              plotBackgroundColor: null,
              backgroundColor: 'rgba(0, 0, 0, 0)',
              height: 300,
              plotBorderWidth: null,
              plotShadow: false,
            },
            colors: ['#E6A4B4', '#F1E4C3', '#FFE382', '#FFAD84'],
            title: {
              text: null,
            },
            plotOptions: {
              pie: {
                dataLabels: {
                  enabled: true,
                  // format: '<b>{point.name}</b>: {point.percentage:.1f}%',
                  distance: 30,
                  style: {
                    color: 'black', // Set the text color to black
                  },
                },
                enableMouseTracking: false,
              },
            },
            legend: {
              layout: 'vertical',
              verticalAlign: 'middle',
              itemStyle: {
                width: 150,
              },
              itemMarginTop: 10,
              itemMarginBottom: 10,
              labelFormatter: function () {
                return `${this.name}: ${this.y}`;
              },
            },
            ...konfigurasiLegend,
            series: [{
              type: 'pie',
              colorByPoint: true,
              innerSize: '80%',
              data: [{
                  name: 'Critical',
                  y: 90
                },
                {
                  name: 'Low',
                  y: 80
                },
                {
                  name: 'Moderate',
                  y: 50
                },
                {
                  name: 'Important',
                  y: 10
                },
              ],
            }, ],
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
  
    .vulner-text {
      position: absolute;
      transform: translateX(-50%);
      font-size: 15px;
      font-style: bold;
      border-radius: 10px;
      margin-top: 25%;
      margin-left: 45%;
    }
  
    .darkmode {
      color: #fff;
    }
  </style>