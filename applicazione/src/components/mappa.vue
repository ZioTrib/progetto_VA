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
    /* const scala = [
      [0, 'rgb(150,0,90)'],
      [0.125, 'rgb(0, 0, 200)'],
      [0.25, 'rgb(0, 25, 255)'],
      [0.375, 'rgb(0, 152, 255)'],
      [0.5, 'rgb(44, 255, 150)'],
      [0.625, 'rgb(151, 255, 0)'],
      [0.75, 'rgb(255, 234, 0)'],
      [0.875, 'rgb(255, 111, 0)'],
      [1, 'rgb(255, 0, 0)']]; */
    return {
      data: [
        {
          type: 'scattermapbox',
          name: [],
          text: [],
          lat: [],
          lon: [],
          key: [],
          nome: [],
          regione: [],
          provincia: [],
          proprietario: [],
          datacomp: [],
          esito: [],
          scopo: [],
          tipo: [],
          uso: [],
          posizione: [],
          stato: [],
          marker: {
            color: [],
            size: 8,
            colorscale: scl,
            cmin: 0,
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
      if (this.selettore === 'PROFONDITA') {
        this.data[0].text = datum.map(d => d.profinfo);
        this.data[0].marker.color = datum.map(d => d.prof);
      } else if (this.selettore === 'LOCALIZZAZIONE') {
        this.data[0].text = datum.map(d => d.geoinfo);
        this.data[0].marker.color = datum.map(d => d.prof);
        this.data[0].nome = datum.map(d => d.regione);
      }
      this.data[0].marker.color = datum.map(d => d.prof);
      this.data[0].key = datum.map(d => d.key);
      this.data[0].nome = datum.map(d => d.nome);
      this.data[0].regione = datum.map(d => d.regione);
      this.data[0].provincia = datum.map(d => d.provincia);
      this.data[0].proprietario = datum.map(d => d.proprietar);
      this.data[0].datacomp = datum.map(d => d.datacomp);
      this.data[0].esito = datum.map(d => d.esito);
      this.data[0].scopo = datum.map(d => d.scopo);
      this.data[0].tipo = datum.map(d => d.tipo);
      this.data[0].uso = datum.map(d => d.uso);
      this.data[0].posizione = datum.map(d => d.posizione);
      this.data[0].stato = datum.map(d => d.stato);
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
