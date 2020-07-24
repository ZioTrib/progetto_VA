<template>
  <vue-plotly :data="data" :layout="layout"/>
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
      mode: 'markers',
      marker: {
        size: 20,
        color: [],
      },
    };
    return {
      data: [trace1],
      layout: {
        title: 'Scatter Plot with a Color Dimension',
      },
    };
  },
  watch: {
    Aggregation_scatter(datum) {
      this.data[0].y = datum.map(d => d.prof);
      this.data[0].marker.color = datum.map(d => d.temp);
    },
  },
};
</script>

<style scoped>

</style>
