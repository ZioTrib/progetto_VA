<template>
  <b-container class="dashboard">
    <b-row class="text-center">
      <b-col cols="9">
        <h2>Mappa dei pozzi in italia</h2>
        <div style="height:500px; background-color: beige"></div>
      </b-col>
      <b-col cols="3">
        <h2>informazioni del pozzo</h2>
        <div style="height:250px; background-color: beige">
          <informazioni measure="# Records" :value = "numRecords"></informazioni>
        </div>
        <b-col>
          <div>
            <h4>Attributi</h4>
            <div>
              <b-form-group label="Stacked (vertical) switch style checkboxes" class="text-left">
                <b-form-checkbox-group
                  v-model="uso.selected"
                  :options="uso.options"
                  switches
                  stacked
                ></b-form-checkbox-group>
              </b-form-group>
            </div>
            <div>
            </div>
            <div>
            </div>
          </div>
        </b-col>
      </b-col>
    </b-row>

    <b-row class="text-center">
      <b-col>
        <h3> quota e profondità pozzi </h3>
        <div style="height:600px; background-color: beige">
        <chart :cfAggregation="pozzi"></chart>
        </div>
      </b-col>
    </b-row>
    <b-row class="text-center">
      <b-col>
        <h3> 1</h3>
        <div style="height:250px; background-color: beige"></div>
      </b-col>
      <b-col>
        <h3> 2 </h3>
        <div style="height:250px; background-color: beige"></div>
      </b-col>
      <b-col>
        <h3> 3 </h3>
        <div style="height:250px; background-color: beige"></div>
      </b-col>
    </b-row>
  </b-container>
</template>


<script>
import crossfilter from 'crossfilter';
import informazioni from './informazioni';
import chart from './chart';

// crossfilter data management
let cf; // crossfilter instance
let duso;


export default {
  name: 'dashboard',
  components: {
    informazioni,
    chart,
  },
  data() {
    return {
      uso: {
        selected: 'TUTTI',
        options: ['TUTTI'],
      },
      numRecords: 0,
      reports: [],
      pozzi: [],
    };
  },

  mounted() {
    fetch('static/data/pozzi.json')
      .then(res => res.json())
      .then((out) => {
        this.reports = out.map(d => ({
          uso: d.uso,
          prof: +d.prof,
          nome: d.nome,
        }));

        cf = crossfilter(this.reports);
        duso = cf.dimension(d => d.uso);

        cf.groupAll().reduceCount().value();
        this.uso.options = ['TUTTI'].concat(duso.group().reduceCount().all().map(v => v.key));
        this.uso.selected = this.uso.options[0];
        this.refreshCounters();
        this.refreshCharts();
      });
  },
  methods: {
    refreshCounters() {
      this.numRecords = cf.groupAll().reduceCount().value();
    },
    refreshCharts() {
      /* eslint-disable */
      this.pozzi = this.reports.map(v => ({ key: v.nome, value: +v.prof }));
    },
  },
  watch: {
    uso: {
      handler(newVal) {
        if (newVal.selected === 'TUTTI') {
          duso.filter(null);
        } else {
          duso.filter(newVal.selected);
        }
        this.refreshCounters();
        this.refreshCharts();
      },
      deep: true, // force watching within properties
    },
  },
};
</script>

<style scoped>

</style>
