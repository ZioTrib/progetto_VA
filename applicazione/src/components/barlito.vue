<template>
  <div class="chartElem">
    <div class="row">
      <highcharts class="chart" :constructor-type="'chart'" :options="chartOptions"></highcharts>
    </div>
  </div>
</template>

<script>
import Highcharts from 'highcharts';
import dataModule from 'highcharts/modules/data';

dataModule(Highcharts);
const chartOptions = {
  chart: {
    width: 560,
    backgroundColor: 'rgb(248,249,250)',
    type: 'column',
  },
  credits: {
    enabled: false,
  },
  title: {
    text: null,
  },
  xAxis: {
    opposite: true,
    categories: [],
    title: {
      enabled: false,
      // text: 'Litho-Stratigraphic Pile'
    },
    startOnTick: true,
    endOnTick: true,
    showLastLabel: true,
  },
  yAxis: {
    reversed: true,
    min: 0,
    title: {
      text: 'profondità',
      style: {
        color: 'rgb(68,68,68)',
        fontSize: '18px',
      },
    },
    /*    stackLabels: {
      enabled: true,
      inside: false,
      y: 0,
      style: {
        fontWeight: 'bold',
        color: (Highcharts.theme && Highcharts.theme.textColor) || 'blue',
      },
    }, */
    labels: {
      style: {
        color: 'rgb(68,68,68)',
      },
    },
  },
  legend: {
    enabled: false,
    // layout: 'horizontal',
    // align: 'center',
    // verticalAlign: 'top',
    // x: 0,
    // y: 100,
    // floating: true,
    // backgroundColor: (Highcharts.theme && Highcharts.theme.legendBackgroundColor) || '#FFFFFF',
    // borderWidth: 1
  },
  plotOptions: {
    series: {
      pointWidth: 100,
      stacking: 'normal',
      colorByPoint: true,
      inside: true,
      dataLabels: {
        enabled: true,
        color: (Highcharts.theme && Highcharts.theme.dataLabelsColor) || 'white',
        style: {
          // textShadow: '0 0 3px black',
          color: '#606060',
          fontSize: '9px',
          width: 50,
        },
        // formatter: function () {
        // return '<b>' + this.point.name + '</b>' + ': ' + this.y + '<br/>' ;
        // return '<b>' + this.point.name + '</b>' ;
        // },
        crop: true,
        overflow: 'none',
        // borderRadius: 5,
        // backgroundColor: 'rgba(125, 125, 125, 0.7)',
        // borderWidth: 1,
        // borderColor: '#AAA',
        // inside: true,
        // x: 100
      },
    },
  },
  tooltip: {
    formatter() {
      return `<b>${this.point.name
      }</b>, profondità sezione: <b>${this.y} m</b>`;
    },
  },
  series: [{
    data: [],
  }],
};
export default {
  props: {
    Aggregation_bar: Array,
  },
  data() {
    return {
      chartOptions,
    };
  },
  mounted() {
    this.$children[0].chart.vueRef = this;
  },
  watch: {
    Aggregation_bar(datum) {
      this.chartOptions.series[0].data = datum;
      this.chartOptions.xAxis.categories = datum.map(d => d.well);
    },
  },
};
</script>

