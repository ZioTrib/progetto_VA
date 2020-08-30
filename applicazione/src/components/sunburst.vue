<template>
  <sunburst :data="tree" :colorScheme="colorScheme" :minAngleDisplayed="minAngleDisplayed">

    <!-- Add behaviors -->
    <template slot-scope="{ on, actions }">
      <highlightOnHover v-bind="{ on, actions }"/>
      <zoomOnClick v-bind="{ on, actions }"/>
    </template>

    <!-- Add information to be displayed on top the graph -->
    <nodeInfoDisplayer slot="top"
                       slot-scope="{ nodes }"
                       :current="nodes.mouseOver"
                       :root="nodes.root"
                       description="di tutti i pozzi"/>

    <!-- Add bottom legend -->
    <breadcrumbTrail slot="legend"
                     slot-scope="{ nodes, colorGetter, width }"
                     :current="nodes.mouseOver"
                     :root="nodes.root"
                     :colorGetter="colorGetter"
                     :from="nodes.clicked"
                     :width="width"
                     :item-width="258"
                     :item-height="45"/>

  </sunburst>
</template>

<script>
import {
  breadcrumbTrail,
  highlightOnHover,
  nodeInfoDisplayer,
  sunburst,
  zoomOnClick,
} from 'vue-d3-sunburst';
import { colorSchemes } from 'vue-d3-sunburst/src/infra/colorSchemes';
import 'vue-d3-sunburst/dist/vue-d3-sunburst.css';


const colorSchemesNames = Object.keys(colorSchemes)
  .map(key => ({
    value: key,
    text: colorSchemes[key].name
  }));

export default {
  props: {
    datisunburst: Array,
  },
  components: {
    breadcrumbTrail,
    highlightOnHover,
    nodeInfoDisplayer,
    sunburst,
    zoomOnClick,
  },
  data() {
    return {
      colorScheme: colorSchemesNames[6].value,
      colorSchemes: colorSchemesNames,
      minAngleDisplayed: 0.00,
      tree: {
        name: 'POZZI',
        children: [
          {
            name: 'scopo: IDROCARBURI',
            children: [
              {
                name: 'uso: NESSUNO',
                children: [
                  { name: 'CEMENTATO', size: 44 },
                  { name: 'NON INDICATO', size: 25 },
                  { name: 'CHIUSO TEMPORANEAMENTE', size: 1 },
                  { name: 'TAPPATO E ABBANDONATO', size: 11 },
                  { name: 'ABBANDONATO', size: 6 },
                ],
              },
              {
                name: 'uso: NON INDICATO',
                children: [
                  { name: 'stato: CEMENTATO', size: 105 },
                  { name: 'stato: IN PRODUZIONE', size: 4 },
                  { name: 'stato: NON INDICATO', size: 2107 },
                  { name: 'stato: CHIUSO TEMPORANEAMENTE', size: 8 },
                  { name: 'stato: CHIUSO MINERARIAMENTE', size: 75 },
                  { name: 'stato: TAPPATO E ABBANDONATO', size: 106 },
                  { name: 'stato: ESAURITO', size: 8 },
                  { name: 'stato: ABBANDONATO', size: 77 },
                  { name: 'stato: COMPLETAMENTO DOPPIO', size: 1 },
                ],
              },
            ],
          },
          {
            name: 'scopo: GEOTERMICO',
            children: [
              {
                name: 'uso: NESSUNO',
                children: [
                  { name: 'CEMENTATO', size: 13 },
                  { name: 'NON INDICATO', size: 2 },
                  { name: 'CHIUSO TEMPORANEAMENTE', size: 115 },
                  { name: 'IN REINIEZIONE', size: 2 },
                  { name: 'OSTRUITO', size: 1 },
                ],
              },
              {
                name: 'uso: NON INDICATO',
                children: [
                  { name: 'stato: IN PRODUZIONE', size: 278 },
                  { name: 'stato: NON INDICATO', size: 106 },
                  { name: 'stato: CHIUSO TEMPORANEAMENTE', size: 261 },
                  { name: 'stato: CHIUSO MINERARIAMENTE', size: 1 },
                  { name: 'stato: ABBANDONATO', size: 1 },
                ],
              },
              {
                name: 'uso: CONTROLLO',
                children: [
                  { name: 'stato: NON INDICATO', size: 9 },
                  { name: 'stato: CHIUSO TEMPORANEAMENTE', size: 17 },
                ],
              },
              {
                name: 'uso: ENERGIA ELETTRICA',
                children: [
                  { name: 'stato: IN PRODUZIONE', size: 45 },
                  { name: 'stato: CHIUSO TEMPORANEAMENTE', size: 5 },
                ],
              },
              {
                name: 'uso: BALNEOTERAPICO',
                children: [
                  { name: 'stato: NON INDICATO', size: 2 },
                ],
              },
              {
                name: 'uso: AGROZOOTECNICO',
                children: [
                  { name: 'stato: IN PRODUZIONE', size: 1 },
                  { name: 'stato: NON INDICATO', size: 2 },
                ],
              },
              {
                name: 'uso: PROCESSI INDUSTRIALI',
                children: [
                  { name: 'stato: NON INDICATO', size: 2 },
                  { name: 'stato: NON CHIUSO TEMPORANEAMENTE', size: 2 },
                ],
              },
              {
                name: 'uso: STOCCAGGIO',
                children: [
                  { name: 'stato: NON CHIUSO TEMPORANEAMENTE', size: 3 },
                ],
              },
            ],
          },
          {
            name: 'scopo: NON INDICATO',
            children: [
              {
                name: 'uso: NESSUNO',
                children: [
                  { name: 'NON INDICATO', size: 4 },
                  { name: 'CHIUSO MINERARIAMENTE', size: 3 },
                  { name: 'TAPPATO E ABBANDONATO', size: 15 },
                ],
              },
              {
                name: 'uso: NON INDICATO',
                children: [
                  { name: 'stato: IN PRODUZIONE', size: 2 },
                  { name: 'stato: NON INDICATO', size: 272 },
                  { name: 'stato: CHIUSO MINERARIAMENTE', size: 3 },
                  { name: 'stato: TAPPATO E ABBANDONATO', size: 13 },
                  { name: 'stato: ABBANDONATO', size: 4 },
                ],
              },
              {
                name: 'uso: CONTROLLO',
                children: [
                  { name: 'stato: IN PROVA', size: 1 },
                ],
              },
              {
                name: 'uso: BALNEOTERAPEUTICO',
                children: [
                  { name: 'stato: NON INDICATO', size: 3 },
                ],
              },
            ],
          },
          {
            name: 'scopo: MINERARIO',
            children: [
              {
                name: 'uso: NESSUNO',
                children: [
                  { name: 'NON INDICATO', size: 1 },
                  { name: 'TAPPATO E ABBANDONATO', size: 1 },
                ],
              },
              {
                name: 'uso: NON INDICATO',
                children: [
                  { name: 'stato: NON INDICATO', size: 11 },
                ],
              },
              {
                name: 'uso: POTABILE',
                children: [
                  { name: 'stato: NON INDICATO', size: 1 },
                ],
              },
            ],
          },
          {
            name: 'scopo: ACQUA FREDDA',
            children: [
              {
                name: 'uso: NESSUNO',
                children: [
                  { name: 'NON INDICATO', size: 2 },
                ],
              },
              {
                name: 'uso: NON INDICATO',
                children: [
                  { name: 'stato: NON INDICATO', size: 12 },
                ],
              },
              {
                name: 'uso: CONTROLLO',
                children: [
                  { name: 'stato: IN PROVA', size: 1 },
                ],
              },
              {
                name: 'uso: POTABILE',
                children: [
                  { name: 'stato: IN PRODUZIONE', size: 1 },
                  { name: 'stato: NON INDICATO', size: 1 },
                ],
              },
              {
                name: 'uso: BALNEOTERAPEUTICO',
                children: [
                  { name: 'stato: NON INDICATO', size: 8 },
                ],
              },
              {
                name: 'uso: PROCESSI INDUSTRIALI',
                children: [
                  { name: 'stato: CHIUSO TEMPORANEAMENTE', size: 1 },
                ],
              },
            ],
          },
        ],
      },
    };
  },
};

</script>

<style scoped>

</style>
