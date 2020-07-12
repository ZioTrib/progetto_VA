<template>
    <vue-plotly :data="data" :layout="layout" :options="options"/>
</template>

<script>
import VuePlotly from '@statnett/vue-plotly';

export default {
  name: 'chart',
  components: {
    VuePlotly,
  },
  props: {
    cfAggregation: Array,
  },
  data() {
    const trace1 = {
      x: [1, 2, 3, 4],
      y: [1, 4, 9, 16],
      name: 'profondità',
      type: 'bar',
    };
    const trace2 = {
      x: [1, 2, 3, 4],
      y: [6, -8, -4.5, 8],
      name: 'quota',
      type: 'bar',
    };
    return {
      data: [trace1, trace2],
      layout: {
        width: 1200,
        heigth: 600,
        barmode: 'relative',
        xaxis: {
          type: 'category',
        },
      },
      options: {
      },
    };
  },
  watch: {
    cfAggregation(datum) {
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
