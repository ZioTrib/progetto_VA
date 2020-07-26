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
    type: 'column',
  },
  title: {
    text: 'Litho-Stratigraphic profile',
  },
  xAxis: {
    opposite: true,
    categories: ['Well: LATINA 1'],
    title: {
      enabled: true,
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
      text: 'Depth (m)',
    },
    stackLabels: {
      enabled: true,
      inside: true,
      y: 15,
      style: {
        fontWeight: 'bold',
        color: (Highcharts.theme && Highcharts.theme.textColor) || 'gray',
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
          textShadow: '0 0 3px black',
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
      }</b>, thickness: <b>${this.y} m</b>`;
    },
  },
  series: [{
    name: 'Well: LATINA 1',
    data: [],
    // name: 'LATINA 1'
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
    /* eslint-disable */
    console.log(this.chartOptions.series[0].data);
  },
  watch: {
    Aggregation_bar(datum) {
      this.chartOptions.series[0].data = datum;
    },
  },
};
</script>
