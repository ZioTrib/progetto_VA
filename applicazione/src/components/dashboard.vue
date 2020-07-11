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
              <b-form-group label="Stacked (vertical) switch style checkboxes" class="text-left">
                <b-form-checkbox-group
                  v-model="uso.selected"
                  :options="uso.options"
                  switches
                  stacked
                ></b-form-checkbox-group>
              </b-form-group>
            </div>
          </div>
        </b-col>
      </b-col>
    </b-row>

    <b-row class="text-center">
      <b-col>
        <h3> quota e profondità pozzi </h3>
        <div style="height:400px; background-color: beige"></div>
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

// crossfilter data management
let cf; // crossfilter instance
// let dnome;
// let dprof;
let duso;
// let dVesselType;


export default {
  name: 'dashboard',
  components: {
    informazioni,
  },
  data() {
    return {
      uso: {
        selected: [],
        options: [],
      },
      numRecords: 0,
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
        // duso = cf.dimension(d => d.uso);
        duso = cf.dimension(d => d.uso);
        // dprof = cf.dimension(d => d.prof);
        /* eslint-disable */

        console.log('num reports', cf.groupAll().reduceCount().value());

        this.uso.options = duso.group().reduceCount().all().map(v => v.key);
        this.uso.selected = this.uso.options[0];
        this.refreshCounters();
      });
  },
  methods: {
    refreshCounters() {
      this.numRecords = cf.groupAll().reduceCount().value();
    },
  },
  watch: {
    uso: {
      handler(newVal) {
        duso.filter(newVal.selected);
        this.refreshCounters();
      },
      deep: true, // force watching within properties
    },
  },
};
</script>

<style scoped>

</style>
