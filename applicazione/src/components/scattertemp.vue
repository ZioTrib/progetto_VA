<template>
  <vue-plotly :data="data" :layout="layout" :options="options"/>
</template>

<script>
import VuePlotly from '@statnett/vue-plotly';

export default {
  name: 'scattertemp',
  components: {
    VuePlotly,
  },
  props: {
    Aggregation_scatter: Array,
  },
  data() {
    const trace1 = {
      y: [],
      text: [],
      textposition: 'bottom',
      mode: 'markers+text',
      marker: {
        size: 20,
        color: [],
      },
    };
    return {
      data: [trace1],
      layout: {
        font: {
          family: 'Avenir, sans-serif',
          size: 15,
        },
        paper_bgcolor: 'rgb(248,249,250)',
        plot_bgcolor: 'rgb(248,249,250)',
        dragmode: 'pan',
        height: 470,
        margin: {
          l: 60,
          r: 50,
          b: 30,
          t: 30,
          pad: 0,
        },
        autosize: true,
        hovermode: 'y',
        hoverinfo: 'y',
        xaxis: {
          autorange: true,
          showgrid: false,
          zeroline: false,
          showline: false,
          autotick: true,
          fixedrange: true,
          ticks: '',
          showticklabels: false,
          title: 'misurazioni',
        },
        yaxis: {
          showgrid: true,
          zeroline: false,
          showline: false,
          fixedrange: true,
          title: 'profondità',
        },
      },
      options: {
        displayModeBar: false,
      },
    };
  },
  watch: {
    Aggregation_scatter(datum) {
      this.data[0].y = datum.map(d => d.prof * -1);
      this.data[0].text = datum.map(d => d.text);
      this.data[0].marker.color = datum.map(d => d.temp);
    },
  },
};
</script>

<style scoped>

</style>
