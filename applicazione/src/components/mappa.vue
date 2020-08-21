<template>
  <vue-plotly ref="Plotly" :data="data" :layout="layout" :config="config" :options="options"
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
    datimappa: Array,
    selettore: String,
  },
  data() {
    const scl = [
      [0, '#9EC9FF'],
      [0.125, '#77B5FF'],
      [0.25, '#5AA3FF'],
      [0.375, '#4893FF'],
      [0.5, '#3d84f3'],
      [0.625, '#2670DF'],
      [0.75, '#044BA0'],
      [0.875, '#003B81'],
      [1, '#002B61'],
    ];
    return {
      onmapclickname: 'NESSUNO',
      data: [
        {
          type: 'scattermapbox',
          nome: [],
          text: [],
          lat: [],
          lon: [],
          hoverinfo: 'text',
          marker: {
            color: [],
            size: 15,
            colorscale: scl,
            cmin: 0,
            symbol: 'circle',
            reversescale: false,
            opacity: 0.7,
            colorbar: {
              thickness: 10,
              titleside: 'right',
              outlinecolor: 'rgba(68,68,68,0)',
              ticks: 'outside',
              ticklen: 3,
              shoticksuffix: 'last',
              ticksuffix: ' metri',
            },
          },
        },
      ],
      layout: {
        paper_bgcolor: 'rgb(248,249,250)',
        plot_bgcolor: 'rgb(248,249,250)',
        colorbar: true,
        autosize: true,
        dragmode: 'zoom',
        mapbox: { style: 'carto-darkmatter', center: { lat: 42, lon: 11.8 }, zoom: 4.35 },
        margin: { r: 0, t: 0, b: 0, l: 0 },
      },
      options: {
        displayModeBar: false,
      },
      config: {
        responsive: true,
      },
    };
  },
  methods: {
    emitToParent() {
      this.$emit('maptoDashboard', this.onmapclickname);
    },
  },
  watch: {
    datimappa(datum) {
      this.data[0].nome = datum.map(d => d.nome);
      this.data[0].lon = datum.map(d => d.lon);
      this.data[0].lat = datum.map(d => d.lat);
      this.data[0].marker.color = datum.map(d => d.prof);
      if (this.selettore === 'PROFONDITÀ | QUOTA') {
        this.data[0].text = datum.map(d => d.profinfo);
      } else if (this.selettore === 'LOCALIZZAZIONE') {
        this.data[0].text = datum.map(d => d.geoinfo);
      } else if (this.selettore === 'PROPRIETARIO') {
        this.data[0].text = datum.map(d => d.proprinfo);
      } else if (this.selettore === 'CONDIZIONI') {
        this.data[0].text = datum.map(d => d.condizioniinfo);
      }
      this.$refs.Plotly.$on('click', (data) => {
        for (let i = 0; i < data.points.length; i += 1) {
          this.onmapclickname = data.points[0].data.nome[data.points[0].pointNumber];
        }
      });
    },

    selettore(newVal, oldVal) {
      if (newVal !== oldVal) {
        this.selettore = newVal;
        this.datimappa = newVal;
      }
    },

    onmapclickname(newVal, oldVal) {
      if (newVal !== oldVal) {
        this.onmapclickname = newVal;
      }
      return this.onmapclickname;
    },
    deep: true, // force watching within properties
  },
};
</script>

<style scoped>

</style>
