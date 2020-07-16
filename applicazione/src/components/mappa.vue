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
  },

  data() {
    const scl = [
      [0, 'rgb(150,0,90)'],
      [0.125, 'rgb(0, 0, 200)'],
      [0.25, 'rgb(0, 25, 255)'],
      [0.375, 'rgb(0, 152, 255)'],
      [0.5, 'rgb(44, 255, 150)'],
      [0.625, 'rgb(151, 255, 0)'],
      [0.75, 'rgb(255, 234, 0)'],
      [0.875, 'rgb(255, 111, 0)'],
      [1, 'rgb(255, 0, 0)']];
    return {
      data: [
        {
          type: 'scattermapbox',
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
              ticksuffix: ' metri',
            },
          },
        },
      ],
      layout: {
        colorbar: true,
        dragmode: 'zoom',
        mapbox: { style: 'stamen-terrain', center: { lat: 43, lon: 10 }, zoom: 4 },
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
      this.data[0].text = datum.map(d => d.nome);
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
      this.$refs.Plotly.$on('click', (d) => {
        /* eslint-disable */
        alert(
          `Nome:\n${d.points[0].data.nome[d.points[0].pointNumber]
          }\n\nLatitudine:\n${d.points[0].data.lat[d.points[0].pointNumber]
          }\n\nLongitudine:\n${d.points[0].data.lon[d.points[0].pointNumber]
          }\n\nRegione:\n${d.points[0].data.regione[d.points[0].pointNumber]
          }\n\nProvincia:\n${d.points[0].data.provincia[d.points[0].pointNumber]
          }\n\nPosizione:\n${d.points[0].data.posizione[d.points[0].pointNumber]
          }\n\nProprietario:\n${d.points[0].data.proprietario[d.points[0].pointNumber]
          }\n\nData:\n${d.points[0].data.datacomp[d.points[0].pointNumber]
          }\n\nEsito:\n${d.points[0].data.esito[d.points[0].pointNumber]
          }\n\nScopo:\n${d.points[0].data.scopo[d.points[0].pointNumber]
          }\n\nTipo:\n${d.points[0].data.tipo[d.points[0].pointNumber]
          }\n\nUso:\n${d.points[0].data.uso[d.points[0].pointNumber]
          }\n\nStato:\n${d.points[0].data.stato[d.points[0].pointNumber]}`,
        );
      });
    },
  },
};
</script>

<style scoped>

</style>
