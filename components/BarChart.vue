<!-- components/ChartCard.vue -->
<template>
    <div>
      <div ref="chart"></div>
      <!-- <button @click="openEditModal">Edit Chart</button> -->
    </div>
  </template>
  
  <script>
    import Highcharts from 'highcharts';
  
    export default {
      data() {
        return {
          options: [{
              value: "pie",
              text: "Pie Chart"
            },
            {
              value: "line",
              text: "Line Chart"
            },
            {
              value: "bar",
              text: "Bar Chart"
            },
          ],
          data2: [],
          dataNama: []
        };
      },
      props: {
        data: {
          type: Array,
          default: []
        },
      },
      mounted() {
        const dataUmur = []
        this.data.forEach((element) => {
          dataUmur.push(
            element.umur
          )
          this.dataNama.push(
            element.nama
          )
        })
        this.data2 = [{
          name: 'umur',
          data: dataUmur
        }]
        console.log(this.data)
        console.log(this.data2)
        console.log(this.dataNama)
        this.renderChart();
      },
      methods: {
        renderChart() {
          const chartData = this.chartData
          // Konfigurasi default
          const chartOptions = {
            chart: {
              type: 'bar'
            },
            title: {
              text: 'Historic World Population by Region',
              align: 'left'
            },
            subtitle: {
              text: 'Source: <a ' +
                'href="https://en.wikipedia.org/wiki/List_of_continents_and_continental_subregions_by_population"' +
                'target="_blank">Wikipedia.org</a>',
              align: 'left'
            },
            xAxis: {
              categories: this.dataNama,
              title: {
                text: null
              },
              gridLineWidth: 1,
              lineWidth: 0
            },
            yAxis: {
              min: 0,
              title: {
                text: 'Population (millions)',
                align: 'high'
              },
              labels: {
                overflow: 'justify'
              },
              gridLineWidth: 0
            },
            tooltip: {
              valueSuffix: ' millions'
            },
            plotOptions: {
              bar: {
                borderRadius: '15px',
                dataLabels: {
                  enabled: true
                },
                groupPadding: 0.1
              }
            },
            legend: {
              layout: 'vertical',
              align: 'right',
              verticalAlign: 'top',
              x: -40,
              y: 80,
              floating: true,
              borderWidth: 1,
              backgroundColor: Highcharts.defaultOptions.legend.backgroundColor || '#FFFFFF',
              shadow: true
            },
            credits: {
              enabled: false
            },
            // series: 
            // [{
            //     name: 'Year 1990',
            //     data: [631, 727, 3202, 721, 100, 600]
            // }, 
            // {
            //     name: 'Year 2000',
            //     data: [814, 841, 3714, 726, 200, 700]
            // }, {
            //     name: 'Year 2018',
            //     data: [1276, 1007, 4561, 746, 300, 400]
            // }]
            series: this.data2
          };
  
          // Render chart pada div dengan ref="chart"
          this.chart = Highcharts.chart(this.$refs.chart, chartOptions);
        },
        updateChart() {
          if (this.chart) {
            this.chart.destroy();
            this.renderChart();
          }
          console.log(this.chartTitle)
          console.log(this.chartData)
        },
      },
    };
  
  </script>
  
  <style scoped>
  
  </style>