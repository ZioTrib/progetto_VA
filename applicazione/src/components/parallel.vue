<template>
  <vue-plotly :data="data" :layout="layout" :options="options"/>
</template>

<script>
import VuePlotly from '@statnett/vue-plotly';

export default {
  name: 'parallel',
  components: {
    VuePlotly,
  },
  props: {
    datiparallel: Array,
  },
  data() {
    const trace1 = {
      type: 'parcats',
      dimensions: [
        {
          label: 'TIPO',
          values: [],
        },
        {
          label: 'USO',
          values: [],
        },
        {
          label: 'SCOPO',
          values: [],
        },
        {
          label: 'ESITO',
          values: [],
        },
        {
          label: 'STATO',
          values: [],
        },
      ],
      bundlecolors: true,
      hoverinfo: 'count+probability',
      line: {
        cmin: 0,
        cmax: 1,
        shape: 'hspline',
      },
      labelfont: { size: 15 },
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
        autosize: true,
        height: 560,
      },
      options: {
        displayModeBar: false,
      },
      config: {
        responsive: true,
      },
    };
  },
  watch: {
    datiparallel(datum) {
      this.data[0].dimensions[0].values = datum.map(d => d.tipo);
      this.data[0].dimensions[1].values = datum.map(d => d.uso);
      this.data[0].dimensions[2].values = datum.map(d => d.scopo);
      this.data[0].dimensions[3].values = datum.map(d => d.esito);
      this.data[0].dimensions[4].values = datum.map(d => d.stato);
    },
  },
};

</script>

<style scoped>

</style>
