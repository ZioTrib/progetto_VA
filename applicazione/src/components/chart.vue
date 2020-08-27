<template>
    <vue-plotly :data="data" :layout="layout" :options="options" :config ="config"/>
</template>

<script>
import VuePlotly from '@statnett/vue-plotly';

export default {
  name: 'chart',
  components: {
    VuePlotly,
  },
  props: {
    Aggregation: Array,
  },
  data() {
    const trace1 = {
      x: [],
      y: [],
      text: 'metri',
      base: 0,
      name: 'profondità',
      type: 'bar',
      marker: {
        color: '#002B61',
        line: {
          color: 'rgb(0,0,0)',
          width: 1,
        },
      },
    };
    const trace2 = {
      x: [],
      y: [],
      text: 'metri',
      base: 0,
      name: 'quota',
      type: 'bar',
      marker: {
        color: '#9EC9FF',
        line: {
          color: 'rgb(0,0,0)',
          width: 1,
        },
      },

    };
    return {
      data: [trace1, trace2],
      layout: {
        font: {
          family: 'Avenir, sans-serif',
          size: 15,
        },
        paper_bgcolor: 'rgb(248,249,250)',
        plot_bgcolor: 'rgb(248,249,250)',
        height: 550,
        autosize: true,
        barmode: 'stack',
        dragmode: 'zoom',
        xaxis: {
          tickangle: 45,
          showgrid: false,
          zeroline: false,
          showline: false,
          autotick: true,
          range: [0, 10],
          title: 'nomi pozzi',
          type: 'category',
          rangeslider: {
          },
          base: 0,
        },
        yaxis: {
          showgrid: false,
          zeroline: true,
          showline: false,
          autotick: true,
          fixedrange: true,
          title: 'profondità | quota',
        },
      },
      config: {
        responsive: true,
      },
      options: {
        displayModeBar: false,
      },
    };
  },
  watch: {
    Aggregation(datum) {
      this.data[0].y = datum.map(d => d.value1 * -1);
      this.data[0].x = datum.map(d => d.key);
      this.data[1].y = datum.map(d => d.value2);
      this.data[1].x = datum.map(d => d.key);
    },
  },
};
</script>

<style scoped>

</style>
