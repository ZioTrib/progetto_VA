<template>
  <vue-plotly  ref="Plotly" :data="data" :layout="layout" :config="config"/>
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
      [0, '#d1f3ff'],
      [0.125, '#acddf3'],
      [0.25, '#88c6e9'],
      [0.375, '#66afdf'],
      [0.5, '#4597d5'],
      [0.625, '#2380ca'],
      [0.75, '#0067bd'],
      [0.875, '#004fad'],
      [1, '#00359b']];
    return {
      data: [
        {
          type: 'scattermapbox',
          name: [],
          text: [],
          lat: [],
          lon: [],
          marker: {
            color: [],
            size: 10,
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
        colorbar: true,
        autosize: true,
        dragmode: 'zoom',
        mapbox: { style: 'carto-darkmatter', center: { lat: 42, lon: 11.8 }, zoom: 4.35 },
        margin: { r: 0, t: 0, b: 0, l: 0 },
      },
      config: {
        responsive: true,
      },
    };
  },
  watch: {
    datimappa(datum) {
      this.data[0].lon = datum.map(d => d.lon);
      this.data[0].lat = datum.map(d => d.lat);
      this.data[0].marker.color = datum.map(d => d.prof);
      if (this.selettore === 'PROFONDITÀ/QUOTA') {
        this.data[0].text = datum.map(d => d.profinfo);
      } else if (this.selettore === 'LOCALIZZAZIONE') {
        this.data[0].text = datum.map(d => d.geoinfo);
      } else if (this.selettore === 'PROPRIETARIO') {
        this.data[0].text = datum.map(d => d.proprinfo);
      } else if (this.selettore === 'CONDIZIONI') {
        this.data[0].text = datum.map(d => d.condizioniinfo);
      }
    },
    selettore(newVal, oldVal) {
      if (newVal !== oldVal) {
        this.selettore = newVal;
        this.datimappa = newVal;
      }
    },
    deep: true, // force watching within properties
  },
};
</script>

<style scoped>

</style>
