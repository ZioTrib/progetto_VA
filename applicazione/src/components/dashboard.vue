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
                  max="4000"
                  step="0.01">
                </b-form-input>
                <div class="mt-2">Value: {{ sliderprof.valore }}</div>
              </div>
              <div>
                <b-form-input
                  id="range-2"
                  v-model="sliderquota.valore"
                  type="range"
                  min="0"
                  max="4000"
                  step="0.01">
                </b-form-input>
                <div class="mt-2">Value: {{ sliderquota.valore }}</div>
              </div>
            </b-col>
            <b-col lg="3">
              <div style="height:200px; background-color: beige">
                <informazioni measure="Numero di pozzi: " :value = "numRecords"></informazioni>
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
          <b-row>
            <b-col>
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
import crossfilter from 'crossfilter';
import informazioni from './informazioni';
import chart from './chart';
import mappa from './mappa';

// crossfilter data management
let cf; // crossfilter instance
let dregione;
let dprof;
let dquota;

export default {
  name: 'dashboard',
  components: {
    informazioni,
    chart,
    mappa,
  },
  data() {
    return {
      sliderprof: {
        valore: 4000,
        max: '',
      },
      sliderquota: {
        valore: 4000,
        min: '',
        max: '',
      },
      reports: [],
      selettore: {
        selected: 'top',
        options: [],
      },
      regione: {
        selected: 'TUTTI',
        options: ['TUTTI'],
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
      numRecords: 0,
      datimappa: [],
      chart: {
        profalt: [],
      },
      filters: {
        nome: '',
        quota: '',
        prof: '',
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
        dprof = cf.dimension(d => d.prof);
        dquota = cf.dimension(d => d.quota);

        dprof.filter([0, this.sliderprof.valore]);
        this.regione.options = ['TUTTE'].concat(dregione.group().reduceCount().all().map(v => v.key));
        this.regione.selected = this.regione.options[0];
        this.selettore.options = ['PROFONDITA', 'TEMPERATURA', 'LITOLOGIA'];
        this.selettore.selected = this.selettore.options[0];
        this.refreshCounters();
        this.refreshTable();
      });
  },
  computed: {
    filtered() {
      const filtered = this.tabella.gruppo.filter(item => Object.keys(this.filters).every(key =>
        String(item[key]).includes(this.filters[key])));
      return filtered.length > 0 ? filtered : [{
        nome: '',
        quota: '',
        prof: '',
      }];
    },
  },
  methods: {
    refreshCounters() {
      this.numRecords = cf.groupAll().reduceCount().value();
    },
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
        text: [
          `Nome:${v.nome}`,
          `Regione:${v.regione}`,
          `Provincia:${v.provincia}`,
          `proprietario:${v.proprietar}`,
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
    },
    selectAllRows() {
      this.$refs.selectableTable.selectAllRows();
    },
    clearSelected() {
      this.$refs.selectableTable.clearSelected();
    },
  },
  watch: {
    regione: {
      handler(newVal) {
        if (newVal.selected === 'TUTTE') {
          dregione.filter(null);
        } else {
          dregione.filter(newVal.selected);
        }
        this.refreshCounters();
        this.refreshTable();
      },
      deep: true, // force watching within properties
    },
    sliderprof: {
      handler(newVal, oldVal) {
        if (newVal !== oldVal) {
          this.sliderprof.valore = newVal;
        }
        this.refreshCounters();
        this.refreshTable();
      },
      deep: true, // force watching within properties
    },
    sliderquota: {
      handler(newVal, oldVal) {
        if (newVal !== oldVal) {
          this.sliderquota.valore = newVal;
          dquota.filter([0, newVal]);
        }
        this.refreshCounters();
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
