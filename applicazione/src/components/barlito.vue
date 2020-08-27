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
    width: 460,
    height: 470,
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
    },
    labels: {
      style: {
        fontFamily: 'Avenir, sans-serif',
        fontSize: '15px',
        color: 'rgb(68,68,68)',
      },
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
        fontFamily: 'Avenir, sans-serif',
        fontSize: '18px',
      },
    },
    labels: {
      style: {
        fontFamily: 'Avenir, sans-serif',
        fontSize: '15px',
        color: 'rgb(68,68,68)',
      },
    },
  },
  legend: {
    enabled: false,
  },
  plotOptions: {
    series: {
      pointWidth: 150,
      stacking: 'normal',
      colorByPoint: true,
      inside: true,
      dataLabels: {
        enabled: true,
        color: (Highcharts.theme && Highcharts.theme.dataLabelsColor) || 'white',
        style: {
          color: '#606060',
          fontFamily: 'Avenir, sans-serif',
          fontSize: '13px',
          width: 50,
        },
        crop: true,
        overflow: 'none',
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

