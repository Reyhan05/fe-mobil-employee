<template>
  <div ref="chartRef" style="float: left;"></div>
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
          title: {
            text: null,
          },
          plotOptions: {
            pie: {
              dataLabels: {
                enabled: true,
                format: '<b>{point.name}</b>: {point.percentage:.1f}%',
                distance: -50,
              },
            },
          },
          legend: {
            layout: 'vertical',
            labels: [
              'Missed SLA: 252',
              'Met SLA: 10',
            ],
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
            innerSize: '50%',
            data: [{
                name: 'Met SLA',
                y: 252
              },
              {
                name: 'Missed SLA',
                y: 20
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

</style>
