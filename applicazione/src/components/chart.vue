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
      base: 0,
      name: 'profondità',
      type: 'bar',
      marker: {
      },
    };
    const trace2 = {
      x: [],
      y: [],
      base: 0,
      name: 'quota',
      type: 'bar',
      marker: {
      },
    };
    return {
      data: [trace1, trace2],
      layout: {
        autosize: true,
        barmode: 'stack',
        dragmode: 'pan',
        scrollZoom: true,
        xaxis: {
          range: [2, 15],
          title: 'nomi pozzi',
          type: 'category',
          base: 0,
        },
        yaxis: {
          title: 'profondità / quota',
        },
      },
      config: {
        responsive: true,
      },
      options: {
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
