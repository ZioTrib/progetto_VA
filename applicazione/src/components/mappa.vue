<template>
  <vue-plotly @hover="hover"  :data="data" :layout="layout" :config="config"/>
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
          lon: [],
          lat: [],
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
      config: { responsive: true },
    };
  },
  watch: {
    coordinate(datum) {
      this.data[0].lon = datum.map(d => d.lon);
      this.data[0].lat = datum.map(d => d.lat);
      this.data[0].text = datum.map(d => d.name);
      this.data[0].marker.color = datum.map(d => d.prof);
    },
  },
};
</script>

<style scoped>

</style>
