<template>
    <vue-plotly :data="data" :layout="layout" :options="options" :config ="config"/>
</template>

<script>
import VuePlotly from '@statnett/vue-plotly';
//  TODO: le quote negative devono partire da 0
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
      name: 'profondità',
      type: 'bar',

    };
    const trace2 = {
      x: [],
      y: [],
      name: 'quota',
      type: 'bar',

    };
    return {
      data: [trace1, trace2],
      layout: {
        heigth: 600,
        barmode: 'relative',
        dragmode: 'pan',
        scrollZoom: true,
        xaxis: {
          type: 'category',
        },
      },
      config: {
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
