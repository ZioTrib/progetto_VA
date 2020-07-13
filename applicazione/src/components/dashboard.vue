<template>
  <b-container class="dashboard">
    <b-row class="text-center">
      <b-col cols="9">
        <h2>Mappa dei pozzi in italia</h2>
        <div style="height:400px; background-color: beige"></div>
      </b-col>
      <b-col cols="3">
        <h2>informazioni del pozzo</h2>
        <div style="height:100px; background-color: beige">
          <informazioni measure="# Records" :value = "numRecords"></informazioni>
        </div>
        <b-col>
          <div>
            <h4>Attributi</h4>
            <div>
              <b-form-select v-model="uso.selected" :options="uso.options"></b-form-select>
              <div class="mt-3">Selected: <strong>{{ uso.selected }}</strong></div>
            </div>
            <div>
              <b-form-select v-model="scopo.selected" :options="scopo.options"></b-form-select>
              <div class="mt-3">Selected: <strong>{{ scopo.selected }}</strong></div>
            </div>
            <div>
              <b-form-select v-model="tipo.selected" :options="tipo.options"></b-form-select>
              <div class="mt-3">Selected: <strong>{{ tipo.selected }}</strong></div>
            </div>
          </div>
        </b-col>
      </b-col>
    </b-row>
    <b-row class="text-center">
      <b-col>
        <h3> elenco pozzi </h3>
        <div>
          <b-form-group label="Selection mode:" label-cols-md="4">
            <b-form-select v-model="selectMode" :options="modes" class="mb-3"></b-form-select>
          </b-form-group>
          <b-table
            ref="selectableTable"
            selectable
            :select-mode="selectMode"
            :items="gruppo"
            :fields="fields"
            @row-selected="onRowSelected"
            responsive="sm"
          >
            <!-- Example scoped slot for select state illustrative purposes -->
            <template v-slot:cell(selected)="{ rowSelected }">
              <template v-if="rowSelected">
                <span aria-hidden="true">&check;</span>
                <span class="sr-only">Selected</span>
              </template>
              <template v-else>
                <span aria-hidden="true">&nbsp;</span>
                <span class="sr-only">Not selected</span>
              </template>
            </template>
          </b-table>
          <p>
            Selected Rows:<br>
            {{ selected }}
          </p>
        </div>
      </b-col>
      <b-col>
        <h3> quota e profondità pozzi </h3>
        <div style="height:500px; background-color: beige">
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
let dscopo;
let dtipo;


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
      scopo: {
        selected: 'TUTTI',
        options: ['TUTTI'],
      },
      tipo: {
        selected: 'TUTTI',
        options: ['TUTTI'],
      },
      selected: [],
      modes: ['multi', 'single'],
      fields: ['nome', 'prof', 'quota'],
      selectMode: 'multi',
      numRecords: 0,
      reports: [],
      pozzi: [],
      gruppo: [],
    };
  },

  mounted() {
    fetch('static/data/pozzi.json')
      .then(res => res.json())
      .then((out) => {
        this.reports = out.map(d => ({
          uso: d.uso,
          tipo: d.tipo,
          prof: +d.prof,
          nome: d.nome,
          quota: +d.quota,
          scopo: d.scopo,
        }));

        cf = crossfilter(this.reports);
        duso = cf.dimension(d => d.uso);
        dscopo = cf.dimension(d => d.scopo);
        dtipo = cf.dimension(d => d.tipo);

        cf.groupAll().reduceCount().value();

        this.uso.options = ['TUTTI'].concat(duso.group().reduceCount().all().map(v => v.key));
        this.uso.selected = this.uso.options[0];
        this.scopo.options = ['TUTTI'].concat(dscopo.group().reduceCount().all().map(v => v.key));
        this.scopo.selected = this.scopo.options[0];
        this.tipo.options = ['TUTTI'].concat(dtipo.group().reduceCount().all().map(v => v.key));
        this.tipo.selected = this.tipo.options[0];
        this.refreshCounters();
        this.refreshCharts();
      });
  },
  methods: {
    refreshCounters() {
      this.numRecords = cf.groupAll().reduceCount().value();
    },
    refreshCharts() {
      this.gruppo = this.reports.filter(selected =>
        selected.uso === this.uso.selected &&
        selected.scopo === this.scopo.selected &&
        selected.tipo === this.tipo.selected);
    },

    onRowSelected(items) {
      this.selected = items;
      this.pozzi = this.selected.map(v => ({ key: v.nome, value1: +v.prof, value2: +v.quota }));
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
    scopo: {
      handler(newVal) {
        if (newVal.selected === 'TUTTI') {
          dscopo.filter(null);
        } else {
          dscopo.filter(newVal.selected);
        }
        this.refreshCounters();
        this.refreshCharts();
      },
      deep: true, // force watching within properties
    },
    tipo: {
      handler(newVal) {
        if (newVal.selected === 'TUTTI') {
          dtipo.filter(null);
        } else {
          dtipo.filter(newVal.selected);
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
