<template>
  <vue-plotly ref="Plotlychart" :data="data" :layout="layout" :options="options" :config="config"
              v-on:click="emitToParent"/>
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
      },

    };
    return {
      onchartclickname: 'NESSUN POZZO SELEZIONATO',
      data: [trace1, trace2],
      layout: {
        font: {
          family: 'Avenir, sans-serif',
          size: 15,
        },
        paper_bgcolor: 'rgb(248,249,250)',
        plot_bgcolor: 'rgb(248,249,250)',
        height: 550,
        width: 690,
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
          rangeslider: {},
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
  methods: {
    emitToParent() {
      this.$emit('charttoDashboard', this.onchartclickname);
    },
  },
  watch: {
    Aggregation(datum) {
      this.data[0].y = datum.map(d => d.value1 * -1);
      this.data[0].x = datum.map(d => d.key);
      this.data[1].y = datum.map(d => d.value2);
      this.data[1].x = datum.map(d => d.key);
      this.$refs.Plotlychart.$on('click', (data) => {
        for (let i = 0; i < data.points.length; i += 1) {
          this.onchartclickname = data.points[0].x;
        }
      });
    },
    onchartclickname(newVal, oldVal) {
      if (newVal !== oldVal) {
        this.onchartclickname = newVal;
      }
      this.emitToParent();
      return this.onchartclickname;
    },
    deep: true, // force watching within properties
  },
};
</script>

<style scoped>

</style>
