<template>
  <vue-plotly :data="data" :layout="layout"/>
</template>

<script>
import VuePlotly from '@statnett/vue-plotly';

export default {
  name: 'chart',
  components: {
    VuePlotly,
  },
  props: {
    coordinate: Array,
  },

  data() {
    const scl =
      [[0, 'rgb(5, 10, 172)'],
        [0.35, 'rgb(40, 60, 190)'],
        [0.5, 'rgb(70, 100, 245)'],
        [0.6, 'rgb(90, 120, 245)'],
        [0.7, 'rgb(106, 137, 247)'],
        [1, 'rgb(220, 220, 220)']];
    return {
      data: [{
        type: 'scattermapbox',
        lon: [],
        lat: [],
        hoverinfor: [],
        text: [],
        mode: 'markers',
        marker: {
          size: 8,
          opacity: 0.8,
          reversescale: true,
          autocolorscale: false,
          symbol: 'square',
          line: {
            width: 1,
            color: 'rgb(102,102,102)',
          },
          colorscale: scl,
          cmin: 0,
          color: [],
          colorbar: {
            title: 'Incoming Flights February 2011',
          },
        },
      }],
      layout: {
        title: 'Most Trafficked US airports',
        colorbar: true,
        dragmode: 'zoom',
        // eslint-disable-next-line max-len
        // "open-street-map", "carto-positron", "carto-darkmatter", "stamen-terrain", "stamen-toner" or "stamen-watercolor"
        mapbox: { style: 'stamen-terrain', center: { lat: 43, lon: 10 }, zoom: 4 },
        margin: { r: 0, t: 0, b: 0, l: 0 },
      },
    };
  },
  watch: {
    coordinate(datum) {
      this.data[0].lon = datum.map(d => d.lon);
      this.data[0].lat = datum.map(d => d.lat);
    },
  },
};
</script>

<style scoped>

</style>
