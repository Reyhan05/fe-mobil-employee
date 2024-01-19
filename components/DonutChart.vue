<template>
  <div class="chart-container">
    <div ref="chartRef" style="float: left;"></div>
    <div class="vulner-text">Vulnerabilities</div>
  </div>
</template>

<script>
  import Highcharts from 'highcharts';

  export default {
    data() {
      return {
        chart: null,
      };
    },
    methods: {
      initChart() {
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
          subtitle: {
            useHTML: true,
            text: '<span class="number" style="font-size: 30px"><b>20</b></span>',
            floating: false,
            verticalAlign: 'middle',
            y: -5,
            x: -0
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
    bottom: 55%; /* Adjust as needed */
    left: 50%;
    right: 40%;
    transform: translateX(-50%);
    font-size: 20px;
    border-radius: 10px;
  }
</style>
