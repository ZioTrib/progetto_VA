<template>
  <b-container class="dashboard">
    <b-row class="text-center">
      <b-col cols="9">
        <h2>Mappa dei pozzi in italia</h2>
        <div style="height:500px; background-color: beige"></div>
      </b-col>
      <b-col cols="3">
        <h2>informazioni del pozzo</h2>
        <div style="height:250px; background-color: beige"></div>
        <b-col>
          <div>
            <h4>Attributi</h4>
            <b-form-select v-model="uso.selected" :options="uso.options"></b-form-select>
            <div class="mt-3">tipo di utilizzo: <strong>{{ selected }}</strong></div>
            <b-form-select v-model="selected" :options="options"></b-form-select>
            <div class="mt-3">tipo di utilizzo: <strong>{{ selected }}</strong></div>
            <b-form-select v-model="selected" :options="options"></b-form-select>
            <div class="mt-3">tipo di utilizzo: <strong>{{ selected }}</strong></div>
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

// crossfilter data management
let cf; // crossfilter instance
let dnome;
let dprof;
let duso;
// let dVesselType;


export default {
  name: 'dashboard',
  components: {
  },
  data() {
    return {
      uso: {
        selected: '1',
        options: ['1', '2', '3'],
        reports: [],
      },
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
        dnome = cf.dimension(d => d.nome);
        dprof = cf.dimension(d => d.prof);
        duso.filter('AGROZOOTECNICO');
        /* eslint-disable */
        console.log('uso agro', duso.group().reduceCount().all());
        console.log('key', dnome.group().reduceCount().all());
        console.log('recordType', dprof.group().reduceCount().all());
        /* eslint-disable */
        this.uso.options = duso.group().reduceCount().all().map(v => v.key);
        this.uso.value = this.uso.options[0];
      });
  },

};
</script>

<style scoped>

</style>
