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
    return {
      data: [{
        type: 'bar',
        x: [1, 3],
        y: [2, 4],
      }],
      layout: {
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
      this.data[0].y = datum.map(d => d.value * -1);
      this.data[0].x = datum.map(d => d.key);
    },
  },
};
</script>

<style scoped>

</style>
