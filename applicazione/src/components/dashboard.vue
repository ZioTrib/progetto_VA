<template>
  <b-container class="dashboard">
    <div>
      <b-button v-b-toggle.elencopozzi variant="primary" class="m-1">Elenco dei pozzi</b-button>
      <b-collapse  visible id="elencopozzi" class="m-1">
        <b-card>
          <b-row>
            <b-col lg="6" class="my-1">
              <b-form-group label="Tipo di Selezione:" label-cols-md="4">
                <b-form-select
                  v-model="tabella.selectMode" :options="tabella.modes">
                </b-form-select>
              </b-form-group>
              <div>
                <b-form-select
                  v-model="regione.selected"
                  :options="regione.options"></b-form-select>
                <div class="mt-3">Selected: <strong>{{ regione.selected }}</strong></div>
              </div>
              <div>
                <b-form-input
                  id="range-2"
                  v-model="sliderprof.valore"
                  type="range"
                  min="0"
                  :max= "sliderprof.max"
                  step="0.5">
                </b-form-input>
                <div class="mt-2">Value: {{ sliderprof.valore }}</div>
              </div>
              <div>
                <b-form-input
                  id="range-2"
                  v-model="sliderquota.valore"
                  type="range"
                  :min="sliderquota.min"
                  :max="sliderquota.max"
                  step="0.5">
                </b-form-input>
                <div class="mt-2">Value: {{ sliderquota.valore }}</div>
              </div>
            </b-col>
            <b-col lg="3">
              <div style="height:200px; background-color: beige">
                <informazioni
                  measure = "Numero di pozzi: " :value = "numberofrecords" ></informazioni>
              </div>
            </b-col>
            <b-col lg="3">
              <div style="height:200px; background-color: beige">
                <informazioni
                  measure="Numero di pozzi selezionati: "
                  :value = "tabella.selezionati"></informazioni>
              </div>
            </b-col>
          </b-row>
          <b-row>
            <b-col>
              <b-table
                selectable
                :select-mode="tabella.selectMode"
                @row-selected="onRowSelected"
                responsive="sm"
                sticky-header
                :fields="tabella.fields"
                class='tabella'
                ref="selectableTable"
                striped show-empty :items="filtered">
                <template slot="top-row" slot-scope="{ fields }">
                  <td v-for="field in fields" :key="field.key">
                    <input v-model="filters[field.key]" :placeholder="field.label">
                  </td>
                </template>
                <template v-slot:cell(selezionato)="{ rowSelected }">
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
                <b-button size="sm" @click="selectAllRows">Seleziona Tutto</b-button>
                <b-button size="sm" @click="clearSelected">Deseleziona Tutto</b-button>
              </p>
            </b-col>
          </b-row>
        </b-card>
      </b-collapse>
    </div>
    <div>
      <b-button v-b-toggle.mappa variant="primary" class="m-1"> Mappa dei pozzi in Italia</b-button>
      <b-collapse id="mappa" class="mt-1">
        <b-card>
          <b-row class="text-center">
            <b-col>
              <div style="height:475px; background-color: beige">
                <mappa :datimappa="datimappa" :selettore = "selettore.selected"></mappa>
              </div>
            </b-col>
          </b-row>
          <b-button v-b-toggle.collapse-1-inner size="sm">visualizzazione</b-button>
          <b-collapse id="collapse-1-inner" class="mt-1">
            <b-card>
              <b-row>
                <b-col>
              <b-form-group>
              <b-form-checkbox-group
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
    </div>
    <div>
    <b-button v-b-toggle.prof variant="primary" class="m-1">Profondità e Quota</b-button>
      <b-button v-b-toggle.temperatura variant="primary" class="m-1">Temperatura</b-button>
      <b-button v-b-toggle.litologia variant="primary" class="m-1">Litologia</b-button>
      <b-collapse  id="prof" class="m-1">
          <b-card>
            <h3> quota e profondità pozzi </h3>
              <div style="height:500px; background-color: beige">
                <chart :Aggregation="chart.profalt"></chart>
              </div>
            </b-card>
        </b-collapse>
      <b-collapse  id="temperatura" class="m-1">
        <b-card>
          <h3> Temperatura </h3>
          <b-row>
            <b-col>
              <scattertemp :aggregation_scatter="scatter.temperature"></scattertemp>
            </b-col>
            <b-col>
              <div>
                <b-form-select
                  v-model="pozzo_temp.selected"
                  :options="pozzo_temp.options"></b-form-select>
                <div class="mt-3">Selected: <strong>{{ pozzo_temp.selected }}</strong></div>
              </div>
            </b-col>
          </b-row>
        </b-card>
      </b-collapse>
      <b-collapse  id="litologia" class="m-1">
        <b-card>
          <b-row>
            <b-col>
            </b-col>
          </b-row>
        </b-card>
      </b-collapse>
    </div>
  </b-container>
</template>


<script>
import * as d3 from 'd3';
import crossfilter from 'crossfilter';
import informazioni from './informazioni';
import chart from './chart';
import mappa from './mappa';
import Scattertemp from './scattertemp';


// crossfilter data management
let cf; // crossfilter instance
// eslint-disable-next-line camelcase
let cf_temp;
let dregione;
let dtnome;
// let dprof;
// let dquota;

export default {
  name: 'dashboard',
  components: {
    Scattertemp,
    informazioni,
    chart,
    mappa,
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
      filtro: [],
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
      tabella: {
        selectMode: 'multi',
        modes: ['multi', 'single'],
        gruppo: [],
        fields: [
          'selezionato',
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
        this.selettore.options = ['PROFONDITA', 'LOCALIZZAZIONE', 'LITOLOGIA'];
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
        this.filtro = dtnome.filter(this.pozzo_temp.selected);
      });
  },
  computed: {
    filtered() {
      const filtered = this.tabella.gruppo.filter(item => Object.keys(this.filters)
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
      return this.filtered.length;
    },
  },
  methods: {

    refreshTable() {
      if (this.regione.selected === 'TUTTE') {
        this.tabella.gruppo = this.reports.filter(selected =>
          selected.prof <= this.sliderprof.valore);
      } else {
        this.tabella.gruppo = this.reports.filter(selected =>
          selected.regione === this.regione.selected &&
          selected.prof <= this.sliderprof.valore &&
          selected.quota <= this.sliderquota.valore);
      }
    },

    onRowSelected(items) {
      this.filtro = this.reports_temp.filter(d => d.tnome === this.pozzo_temp.selected);
      this.scatter.temperature = this.filtro.map(v => ({
        nome: v.tnome,
        temp: +v.ttemp,
        prof: +v.tprof,
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
        uso: v.uso,
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
        nome: v.nome,
        quota: v.quota,
        scopo: v.scopo,
        key: v.key,
        entitam: v.entitam,
        regione: v.regione,
        provincia: v.provincia,
        proprietar: v.proprietar,
        datacomp: v.datacomp,
        esito: v.esito,
        posizione: v.posizione,
        stato: v.stato,
      }));
      this.tabella.selezionati = this.selectedTable.length;
      this.datitemp_selettore = this.selectedTable.map(v => ({
        nome: v.nome,
      }));
      this.pozzo_temp.options = this.datitemp_selettore.map(d => d.nome);
      this.pozzo_temp.selected = this.pozzo_temp.options[0];
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
