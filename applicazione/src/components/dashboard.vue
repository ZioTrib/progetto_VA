<template>
  <b-container class="dashboard bg-dark" fluid>
    <b-row>
      <b-col cols="7">
        <b-button v-b-toggle.elencopozzi variant="primary" class="m-1">Elenco dei pozzi</b-button>
        <b-button v-b-toggle.mappa variant="primary"
                  class="m-1"> Mappa dei pozzi in Italia</b-button>
        <b-button v-b-toggle.profalt variant="primary"
                  class="m-1">Profondità - altitudine</b-button>
        <b-collapse  visible id="elencopozzi">
          <b-card bg-variant="light">
            <b-row>
              <b-col lg="6" class="my-1">
                <div>
                  <b-form-group label="Regione Selezionata:" label-cols-md="4">
                    <b-form-select
                    v-model="regione.selected"
                    :options="regione.options"></b-form-select>
                  </b-form-group>
                </div>
                <div>
                  <b-form-input
                    id="range-2"
                    v-model="sliderprof.valore"
                    type="range"
                    min="0"
                    :max= "sliderprof.max"
                    step="0.5"></b-form-input>
                  <div class="mt-2">profondità massima: {{ sliderprof.valore }}</div>
                </div>
                <div>
                  <b-form-input
                    id="range-2"
                    v-model="sliderquota.valore"
                    type="range"
                    :min="sliderquota.min"
                    :max="sliderquota.max"
                    step="0.5"></b-form-input>
                  <div class="mt-2"> quota massima: {{ sliderquota.valore }}</div>
                </div>
              </b-col>
              <b-col lg="3">
                <b-card
                  border-variant="primary"
                  header-bg-variant="primary"
                  header-text-variant="white"
                  align="center"
                  header="NUMERO DI POZZI"
                  class="text-center">
                  <b-card-text> <h2>{{ numberofrecords }}</h2>  </b-card-text>
                </b-card>
              </b-col>
              <b-col lg="3">
                <b-card
                  border-variant="primary"
                  header-bg-variant="primary"
                  header-text-variant="white"
                  align="center"
                  header="POZZI SELEZIONATI"
                  class="text-center"><b-card-text> <h2>{{ tabella.selezionati }}</h2></b-card-text>
                </b-card>
              </b-col>
            </b-row>
            <b-row>
              <b-col>
                <b-table
                  selected-variant="primary"
                  head-variant="dark"
                  table-variant="light"
                  hover
                  selectable
                  :select-mode="tabella.selectMode"
                  @row-selected="onRowSelected"
                  responsive="sm"
                  sticky-header
                  :fields="tabella.fields"
                  class='tabella'
                  ref="selectableTable"
                  show-empty :items="filtered">
                  <template slot="top-row" slot-scope="{ fields }">
                    <td v-for="field in fields" :key="field.key">
                      <input v-model="filters[field.key]" :placeholder="field.label">
                    </td>
                  </template>
                </b-table>
                <b-row class="mb-2">
                  <b-col sm="3">
                    <b-form-group
                      label-cols-sm="6"
                      label-align-sm="right"
                      label-size="sm"
                      class="mb-0"
                      label="Tipo di Selezione: ">
                      <b-form-select
                        v-model="tabella.selectMode" :options="tabella.modes">
                      </b-form-select>
                    </b-form-group>
                  </b-col>
                </b-row>
                <b-row>
                  <b-col>
                    <b-button size="sm" @click="selectAllRows">Seleziona Tutto
                      <b-badge variant="light"> {{ numberofrecords }}</b-badge>
                    </b-button>
                    <b-button size="sm" @click="clearSelected">Deseleziona Tutto
                      <b-badge variant="light"> {{ tabella.selezionati }}</b-badge>
                    </b-button>
                  </b-col>
                </b-row>
              </b-col>
            </b-row>
          </b-card>
        </b-collapse>
        <b-collapse id="mappa" class="mt-1">
          <b-card bg-variant="light">
            <b-row class="text-center">
              <b-col>
                <div style="height:460px">
                  <mappa :datimappa="datimappa" :selettore = "selettore.selected"></mappa>
              </div>
            </b-col>
          </b-row>
            <b-button v-b-toggle.collapse-1-inner size="sm">etichetta pozzo</b-button>
            <b-collapse id="collapse-1-inner" class="mt-2">
              <b-card>
                <b-row>
                  <b-col>
                    <b-form-group>
                      <b-form-checkbox-group
                        size="sm"
                        v-model="selettore.selected"
                        :options="selettore.options"
                        name="buttonsSelector"
                        buttons
                      ></b-form-checkbox-group>
                    </b-form-group>
                  </b-col>
                </b-row>
              </b-card>
            </b-collapse>
          </b-card>
        </b-collapse>
        <b-collapse id="profalt" class="mt-1">
        <b-card bg-variant="light">
          <h3> quota e profondità pozzi </h3>
          <div>
            <b-col cols="12">
            <chart :Aggregation="chart.profalt"></chart>
            </b-col>
          </div>
        </b-card>
        </b-collapse>
      </b-col>
      <b-col cols="5">
        <h2>Grafici</h2>
        <b-card bg-variant="light">
          <h3> Temperatura </h3>
          <b-row>
            <b-col>
              <div>
                <scattertemp :aggregation_scatter="scatter.temperature"></scattertemp>
              </div>
              <div>
                <b-form-select
                  v-model="pozzo_temp.selected"
                  :options="pozzo_temp.options"></b-form-select>
                <div class="mt-3">Selected: <strong>{{ pozzo_temp.selected }}</strong></div>
              </div>
            </b-col>
          </b-row>
        </b-card>
        <b-card bg-variant="light" class="mt-1">
          <h3> Litologia </h3>
          <b-row>
            <b-col>
              <div>
                <highcharts :aggregation_bar="bar.litologia"/>
              </div>
            </b-col>
          </b-row>
          <b-row>
          <b-col>
            <div>
              <b-form-select
                v-model="pozzo_lito.selected"
                :options="pozzo_lito.options"></b-form-select>
              <div class="mt-3">Selected: <strong>{{ pozzo_lito.selected }}</strong></div>
            </div>
          </b-col>
          </b-row>
        </b-card>
      </b-col>
    </b-row>
  </b-container>
</template>


<script>
import * as d3 from 'd3';
import crossfilter from 'crossfilter';
import informazioni from './informazioni';
import chart from './chart';
import mappa from './mappa';
import scattertemp from './scattertemp';
import barlito from './barlito';


// crossfilter data management
let cf; // crossfilter instance
// eslint-disable-next-line camelcase
let cf_temp;
// eslint-disable-next-line camelcase
let cf_lito;
let dregione;
let dtnome;
let dlitonome;

export default {
  name: 'dashboard',
  components: {
    scattertemp,
    informazioni,
    chart,
    mappa,
    highcharts: barlito,
  },
  data() {
    return {
      sliderprof: {
        valore: Function,
        max: Function,
      },
      sliderquota: {
        valore: Function,
        min: Function,
        max: Function,
      },
      reports: [],
      reports_temp: [],
      reports_litstr: [],
      filtro_temperature: [],
      filtro_litologia: [],

      selettore: {
        selected: String,
        options: Array,
      },
      regione: {
        selected: 'TUTTE',
        options: [],
      },
      pozzo_temp: {
        selected: String,
        options: Array,
      },
      pozzo_lito: {
        selected: String,
        options: Array,
      },
      tabella: {
        selectMode: 'multi',
        modes: ['multi', 'single'],
        rowSelected: [],
        fields: [
          {
            key: 'nome',
            label: 'Nome Pozzo',
            sortable: true,
          },
          {
            key: 'prof',
            label: 'Profondità',
            sortable: true,
          },
          {
            key: 'quota',
            label: 'Quota',
            sortable: true,
          },
          {
            key: 'tipo',
            label: 'Tipo',
            sortable: true,
          },
          {
            key: 'uso',
            label: 'Uso',
            sortable: true,
          },
          {
            key: 'esito',
            label: 'Esito',
            sortable: true,
          },
          {
            key: 'stato',
            label: 'Stato',
            sortable: true,
          },
        ],
        selezionati: 0,
      },
      filters: {
        nome: '',
        quota: '',
        prof: '',
      },
      datimappa: [],
      chart: {
        profalt: [],
      },
      scatter: {
        temperature: [],
      },
      bar: {
        litologia: [],
      },
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
          lon_wgs84: d.lon_wgs84,
          lat_wgs84: d.lat_wgs84,
          key: +d.key,
          entitam: d.entitam,
          regione: d.regione,
          provincia: d.provincia,
          proprietar: d.proprietar,
          datacomp: d.datacomp,
          esito: d.esito,
          posizione: d.posizione,
          stato: d.stato,
        }));


        cf = crossfilter(this.reports);
        dregione = cf.dimension(d => d.regione);
        // dprof = cf.dimension(d => d.prof);
        // dquota = cf.dimension(d => d.quota);

        this.sliderprof.valore = d3.max(this.reports, d => d.prof);
        this.sliderquota.valore = d3.max(this.reports, d => d.quota);
        this.sliderprof.max = d3.max(this.reports, d => d.prof);
        this.sliderquota.max = d3.max(this.reports, d => d.quota);
        this.sliderquota.min = d3.min(this.reports, d => d.quota);
        this.regione.options = ['TUTTE'].concat(dregione.group().reduceCount().all().map(v => v.key));
        this.regione.selected = this.regione.options[0];
        this.selettore.options = ['PROFONDITÀ/QUOTA', 'LOCALIZZAZIONE', 'PROPRIETARIO', 'CONDIZIONI'];
        this.selettore.selected = this.selettore.options[0];
        this.refreshTable();
      });
    fetch('static/data/pozzi_temp.json')
      .then(res => res.json())
      .then((out) => {
        this.reports_temp = out.map(d => ({
          tprof: +d.prof,
          ttemp: +d.temp,
          tkey: +d.key,
          tnome: d.nome,
        }));
        // eslint-disable-next-line camelcase
        cf_temp = crossfilter(this.reports_temp);
        dtnome = cf_temp.dimension(d => d.tnome);
        this.filtro_temperature = dtnome.filter(this.pozzo_temp.selected);
      });
    fetch('static/data/pozzi_litstr.json')
      .then(res => res.json())
      .then((out) => {
        this.reports_litstr = out.map(d => ({
          prof: +d.aprof,
          lito: d.litologia,
          nomepozzo: d.nome,
        }));
        // eslint-disable-next-line camelcase
        cf_lito = crossfilter(this.reports_litstr);
        dlitonome = cf_lito.dimension(d => d.nomepozzo);
        this.filtro_litologia = dlitonome.filter(this.pozzo_lito.selected);
      });
  },
  computed: {
    filtered() {
      const filtered = this.tabella.rowSelected.filter(item => Object.keys(this.filters)
        .every(key =>
          String(item[key])
            .includes(this.filters[key])));
      return filtered.length > 0 ? filtered : [{
        nome: '',
        quota: '',
        prof: '',
      }];
    },
    numberofrecords() {
      if (this.tabella.rowSelected.length === 0) {
        return 0;
      }
      return this.filtered.length;
    },
  },
  methods: {
    refreshTable() {
      if (this.regione.selected === 'TUTTE') {
        this.tabella.rowSelected = this.reports.filter(selected =>
          selected.prof <= this.sliderprof.valore &&
          selected.prof <= this.sliderquota.valore,
        );
      } else {
        this.tabella.rowSelected = this.reports.filter(selected =>
          selected.regione === this.regione.selected &&
          selected.prof <= this.sliderprof.valore &&
          selected.quota <= this.sliderquota.valore);
      }
    },

    onRowSelected(items) {
      this.filtro_temperature = this.reports_temp.filter(d => d.tnome === this.pozzo_temp.selected);
      this.scatter.temperature = this.filtro_temperature.map(v => ({
        nome: v.tnome,
        temp: +v.ttemp,
        prof: +v.tprof,
      }));
      this.filtro_litologia = this.reports_litstr.filter(d =>
        d.nomepozzo === this.pozzo_lito.selected);
      this.bar.litologia = this.filtro_litologia.map(v => ({
        x: 0,
        y: +v.prof,
        name: v.lito,
      }));
      this.selectedTable = items;
      this.chart.profalt = this.selectedTable.map(v => ({
        key: v.nome,
        value1: +v.prof,
        value2: +v.quota,
      }));
      this.datimappa = this.selectedTable.map(v => ({
        lon: v.lon_wgs84,
        lat: v.lat_wgs84,
        tipo: v.tipo,
        prof: v.prof,
        geoinfo: [
          `Nome:${v.nome}`,
          `Regione:${v.regione}`,
          `Provincia:${v.provincia}`,
          `Località:${v.entitam}`,
        ],
        profinfo: [
          `Nome:${v.nome}`,
          `Profondità:${v.prof}`,
          `Quota:${v.quota}`,
        ],
        proprinfo: [
          `Nome:${v.nome}`,
          `Proprietario:${v.proprietar}`,
        ],
        condizioniinfo: [
          `Nome:${v.nome}`,
          `Esito:${v.esito}`,
          `Stato:${v.stato}`,
          `Uso:${v.uso}`,
          // `Scopo:${v.scopo}`,
          // `Tipo:${v.tipo}`,
        ],
        quota: v.quota,
        key: v.key,
        entitam: v.entitam,
        regione: v.regione,
        provincia: v.provincia,
        posizione: v.posizione,
      }));
      this.tabella.selezionati = this.selectedTable.length;
      this.nome_selettore = this.selectedTable.map(v => ({
        nome: v.nome,
      }));
      this.pozzo_temp.options = this.nome_selettore.map(d => d.nome);
      this.pozzo_temp.selected = this.pozzo_temp.options[0];
      this.pozzo_lito.options = this.nome_selettore.map(d => d.nome);
      this.pozzo_lito.selected = this.pozzo_lito.options[0];
    },
    selectAllRows() {
      this.$refs.selectableTable.selectAllRows();
    },
    clearSelected() {
      this.$refs.selectableTable.clearSelected();
    },
  },
  watch: {
    pozzo_temp: {
      handler(newVal) {
        dlitonome.filter(newVal.selected);
        this.onRowSelected();
      },
      deep: true, // force watching within properties
    },
    pozzo_lito: {
      handler(newVal) {
        dtnome.filter(newVal.selected);
        this.onRowSelected();
      },
      deep: true, // force watching within properties
    },

    regione: {
      handler(newVal) {
        if (newVal.selected === 'TUTTE') {
          dregione.filter(null);
        } else {
          dregione.filter(newVal.selected);
        }
        this.refreshTable();
      },
      deep: true, // force watching within properties
    },
    sliderprof: {
      handler(newVal, oldVal) {
        if (newVal !== oldVal) {
          this.sliderprof.valore = newVal;
        }
        this.refreshTable();
      },
      deep: true, // force watching within properties
    },
    sliderquota: {
      handler(newVal, oldVal) {
        if (newVal !== oldVal) {
          this.sliderquota.valore = newVal;
        }
        this.refreshTable();
      },
      deep: true, // force watching within properties
    },
  },
};
</script>

<style scoped>
.tabella {
  height: 400px;
  overflow: scroll;
  overflow-style: marquee-block;
}
</style>
