<template>
  <div ref="chartRef"></div>
</template>

<script>
  import Highcharts from "highcharts";

  export default {
    props: {
      data: {
        type: Array,
        default: [],
      },
      legendColor: {
        type: String,
        default: "",
      },
    },
    data() {
      return {
        chart: null,
        data2: [],
      };
    },
    methods: {
      async getData(){
        this.dataSLA[0] = parseFloat(this.data.totalSLA.toFixed(2));
        const data = await this.$axios.$get(
            '/list-case?filter=all&page=all&size=10'
          );     
          console.log(data.data); 
      },
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
            layout: "vertical",
            align: "right",
            verticalAlign: "top",
            symbolWidth: 10,  // Lebar simbol (kotak) pada legend
            symbolHeight: 10, // Tinggi simbol (kotak) pada legend
          },
          tooltip: {
            valueSuffix: "",
            pointFormat: '{series.name}: <b>{point.y}</b>',
          },
          accessibility: {
            point: {
              valueSuffix: "%",
            },
          },
          plotOptions: {
            pie: {
              allowPointSelect: true,
              innerSize: "70%",
              cursor: "pointer",
              dataLabels: {
                enabled: false,
                format: '<b>{point.name}</b>: {point.percentage:.1f}%',
                distance: -50,
              },
              showInLegend: true,
            }
          },
          subtitle: {
            useHTML: true,
            text: '<span style="font-size: 30px"><b>98%</b></span>',
            floating: false,
            verticalAlign: 'middle',
            y: 13,
            x: -50
          },
          series: [{
            data: this.data2,
            name: 'Umur', // Menambahkan nama untuk series
          }],
          responsive: {
            rules: [{
              condition: {
                maxWidth: 500,
              },
            }],
          },
        };

        this.chart = Highcharts.chart(this.$refs.chartRef, chartOptions);
      },
    },
    watch: {},
    mounted() {
      this.data.forEach((element) => {
        this.data2.push({
          name: `${element.nama} (${element.umur})`,
          y: element.umur,
        });
      });
      this.initChart();
      console.log(this.data);
      console.log(this.data2);
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