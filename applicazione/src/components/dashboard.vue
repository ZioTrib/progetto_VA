<template>
  <!--DASHBOARD-->
  <b-overlay
    no-center
    :show="show"
    :opacity="0.85"
    rounded="lg">
    <template v-slot:overlay>
      <div id="loading-wrapper">
        <div id="loading-text">CARICAMENTO...</div>
        <div id="loading-content"></div>
      </div>
    </template>
    <b-container class="dashboard bg-dark" fluid>
      <div>
        <b-row class="text-center">
          <b-col cols="12" class="mt-2">
            <!--POPOVERS BUTTON-->
            <b-button pill size="sm" variant="warning" class="px-1"
                      @click="nascondi = !nascondi">
              <b-icon icon="info-circle-fill"></b-icon>
              {{ nascondi ? 'Mostra' : 'Nascondi' }} informazioni
            </b-button>
          </b-col>
        </b-row>
        <b-row>
          <b-col cols="7">
            <!--POPOVERS-->
            <b-popover title="SEZIONE ELENCO POZZI" triggers="hover" variant="warning"
                       placement="right"
                       target="popoverElencoPozzi"
                       :disabled.sync="nascondi">
              Nella sezione <strong>"Elenco Pozzi"</strong> è possibile <strong>consultare</strong>
              l'elenco delle risorse geotermiche e <strong>selezionare</strong> quelle
              da voler analizzare. È possibile <strong>filtrare</strong> l'elenco per
              <strong>regione, profondità massima</strong> e <strong>quota massima</strong>.
              Inoltre è prensente una funzione di <strong>ricerca</strong> per ogni
              colonna.

            </b-popover>
            <b-popover title="SEZIONE MAPPA" triggers="hover" variant="warning"
                       placement="right"
                       target="popoverMappa"
                       :disabled.sync="nascondi">
              Nella sezione <strong>"Mappa dei Pozzi"</strong> è possibile consultare la
              geolocalizzazione dei pozzi selezionati. È possibile selezionate il
              <strong>tipo di etichetta delle risorse geotermiche</strong> presenti sulla mappa
              da visualizzare.
              <strong>Cliccando</strong> sulla risorsa geotermica è possibile visualizzare i
              dati relativi alle <strong>temperature</strong> e alla
              <strong>litostratigrafia</strong>.
            </b-popover>
            <b-popover title="SEZIONE PROFONDITÀ E QUOTA" triggers="hover"
                       variant="warning"
                       placement="right"
                       target="popoverProfQuot"
                       :disabled.sync="nascondi">
              Nella sezione <strong>"Profondità | Quota"</strong> è possibile consultare le
              <strong>profondità</strong> e le <strong>quote</strong> delle risorse geotermiche
              selezionate.
              È possibile visualizzare entrambi i profili oppure visualizzarli singolarmente
              cliccando sulla classe "quota" o sulla classe "profondità".
              <strong>Cliccando</strong> sull'altimetria del pozzo è possibile visualizzare i
              dati relativi alle <strong>temperature</strong> e alla
              <strong>litostratigrafia</strong>.
            </b-popover>
            <b-popover title="SEZIONE TEMPERATURA" triggers="hover" variant="warning"
                       placement="left"
                       target="popoverTemp"
                       :disabled.sync="nascondi">
              Nella sezione <strong>"Temperatura"</strong> è possibile <strong>visualizzare le
              varie misurazioni di temperatura </strong> (in gradi Celsius) delle
              risorge geotermiche a differenti livelli di profondità.
              È possibile <strong>selezionare</strong> la risorsa geotermica tra quelle
              selezionate nella sezione "Elenco Pozzi" per la visualizzazione delle temperature.
            </b-popover>
            <b-popover title="SEZIONE LITOSTRATIGRAFIA" triggers="hover" variant="warning"
                       placement="left"
                       target="popoverLito"
                       :disabled.sync="nascondi">
              Nella sezione <strong>"Litostratigrafia"</strong> è possibile visualizzare
              <strong>gli strati
              litologici</strong> che compongono le risorse geotermiche.
              È possibile <strong>selezionare</strong> la risorsa geotermica tra quelle
              selezionate nella sezione "Elenco Pozzi" per la visualizzazione
              della litostratigrafia.
            </b-popover>
            <b-popover title="SEZIONE ALLUVIAL DIAGRAM" triggers="hover" variant="warning"
                       placement="top"
                       target="popoverParallel"
                       :disabled.sync="nascondi">
              Nella sezione <strong>"Alluvial Diagram"</strong> vengono visualizzate le frequenze
              con cui gli attributi <strong>"tipo", "uso", "scopo", "esito", "stato"</strong>
              si presentano per i pozzi selezionati nella sezione "Elenco Pozzi".
            </b-popover>
            <b-popover title="SEZIONE SUNBURST" triggers="hover" variant="warning"
                       placement="top"
                       target="popoverSunburst"
                       :disabled.sync="nascondi">
              Nella sezione <strong>"Sunburst"</strong> è possibile visualizzare come si
              distribuiscono tutte le risorge geotermiche secondo la
              <strong>gerarchia "scopo" -> "uso" -> "stato"</strong>.
              <strong>Cliccando</strong> su uno degli spicchi del grafico è possibile
              analizzare la sezione.
            </b-popover>
            <b-card class="mt-2" style="height: 760px">
              <b-tabs content-class="mt-3" justified pills card>
                <!--ELENCO POZZI TAB START-->

                <b-tab active>
                  <template v-slot:title>
                    <b-icon icon="table"></b-icon>
                    ELENCO POZZI
                  </template>
                  <b-card id="popoverElencoPozzi" bg-variant="light">
                    <!--COUNTERS-->
                    <b-collapse id="collapse-counters" visible class="mt-2">
                      <b-row>
                        <b-col>
                          <b-card
                            border-variant="primary"
                            header-bg-variant="primary"
                            header-text-variant="white"
                            align="center"
                            style="max-height: 300px"
                            class="text-center">
                            <b-card-text><h1>{{ numberofrecords }}</h1></b-card-text>
                            <template v-slot:header>
                              NUMERO DI POZZI
                            </template>
                          </b-card>
                        </b-col>
                        <b-col>
                          <b-card
                            border-variant="primary"
                            header-bg-variant="primary"
                            header-text-variant="white"
                            align="center"
                            style="max-height: 300px"
                            class="text-center">
                            <b-card-text><h1>{{ tabella.selezionati }}</h1></b-card-text>
                            <template v-slot:header>
                              POZZI SELEZIONATI
                            </template>
                          </b-card>
                        </b-col>
                      </b-row>
                    </b-collapse>
                    <b-row>
                      <b-col lg="12" class="my-1">
                        <b-collapse id="collapse-filters" class="mt-2">
                          <!--SELECTOR FOR REGION-->
                          <div>
                            <b-form-group label="Regione selezionata"
                                          label-for="regione">
                              <b-form-select
                                size="sm"
                                id="regione"
                                v-model="regione.selected"
                                :options="regione.options">
                              </b-form-select>
                            </b-form-group>
                          </div>
                          <!--SLIDER PROF-->
                          <b-form-group label-for="maxprof">
                            <b-input-group>
                              <b-input-group-append is-text>
                                <div style="width: 150px">
                                  Profondità massima
                                </div>
                              </b-input-group-append>
                              <b-form-input
                                id="maxprof"
                                v-model="sliderprof.valore"
                                type="range"
                                number
                                :min="sliderprof.min"
                                :max="sliderprof.max"
                                step="100"
                              ></b-form-input>
                              <b-input-group-append is-text>
                                {{ `${sliderprof.valore.toFixed(2)} metri` }}
                              </b-input-group-append>
                            </b-input-group>
                          </b-form-group>
                          <!--SLIDER QUOTA-->
                          <b-form-group label-for="maxquota">
                            <b-input-group>
                              <b-input-group-append is-text>
                                <div style="width: 150px">
                                  Quota massima
                                </div>
                              </b-input-group-append>
                              <b-form-input
                                id="maxquota"
                                v-model="sliderquota.valore"
                                type="range"
                                number
                                :min="sliderquota.min"
                                :max="sliderquota.max"
                                step="100"
                              ></b-form-input>
                              <b-input-group-append is-text>
                                {{ `${sliderquota.valore.toFixed(2)} metri` }}
                              </b-input-group-append>
                            </b-input-group>
                          </b-form-group>
                        </b-collapse>
                      </b-col>
                    </b-row>

                    <b-row>
                      <b-col>
                        <!--START TABLE-->
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
                            <b-td v-for="field in fields" :key="field.key"
                                  variant="dark">
                              <input v-model="filters[field.key]" :placeholder="field.label">
                            </b-td>
                          </template>
                        </b-table>
                        <!--SELECTION BUTTONS-->
                        <b-row>
                          <b-col cols="8">
                            <b-button variant="primary" size="md" @click="selectAllRows">
                              Seleziona Tutto
                              <b-badge variant="light"> {{ numberofrecords }}</b-badge>
                            </b-button>
                            <b-button variant="primary"
                                      size="md" @click="clearSelected">Deseleziona Tutto
                              <b-badge variant="light"> {{ tabella.selezionati }}</b-badge>
                            </b-button>
                          </b-col>
                          <b-col cols="4">
                            <div align="right">
                              <b-button size="md" v-b-toggle.collapse-filters.collapse-counters>
                                <b-icon icon="funnel"></b-icon>
                                Filtra
                              </b-button>
                            </div>
                          </b-col>
                        </b-row>
                      </b-col>
                    </b-row>
                  </b-card>
                </b-tab>
                <b-tab>
                  <template v-slot:title>
                    <b-icon icon="map"></b-icon>
                    MAPPA DEI POZZI
                  </template>
                  <b-card class="text-center" id="popoverMappa" bg-variant="light">
                    <b-row>
                      <b-col>
                        <div style="height:510px; margin:0 auto;">
                          <!--MAP-->
                          <mappa :datimappa="datimappa" :selettore="selettore.selected"
                                 v-on:maptoDashboard="onmapclick"></mappa>
                        </div>
                      </b-col>
                    </b-row>
                    <!--WELL LABEL SELECTOR FOR MAP VISUALIZATION-->
                    <b-form-group>
                      <b-form-checkbox-group
                        size="md"
                        v-model="selettore.selected"
                        :options="selettore.options"
                        name="buttonsSelector"
                        buttons
                      ></b-form-checkbox-group>
                    </b-form-group>
                  </b-card>
                </b-tab>
                <b-tab>
                  <template v-slot:title>
                    <b-icon icon="arrow-down-up"></b-icon>
                    PROFONDITÀ | QUOTA
                  </template>
                  <b-card id="popoverProfQuot" bg-variant="light" class="text-center">
                    <b-col cols="12">
                      <div style="margin:0 auto;">
                        <chart :Aggregation="chart.profalt"
                               v-on:charttoDashboard="onchartclick"></chart>
                      </div>
                    </b-col>
                  </b-card>
                </b-tab>
              </b-tabs>
            </b-card>
          </b-col>
          <b-col cols="5">
            <b-card class="mt-2" style="height: 760px">
              <b-tabs content-class="mt-3" justified pills card>
                <b-tab active>
                  <template v-slot:title>
                    <b-icon icon="thermometer-half"></b-icon>
                    TEMPERATURA
                  </template>
                  <b-card id="popoverTemp" bg-variant="light">
                    <b-row class="mb-5">
                      <b-col>
                        <div>
                          <!-- SCATTER PLOT FOR TEMPERATURE -->
                          <scattertemp :aggregation_scatter="scatter.temperature"></scattertemp>
                        </div>
                      </b-col>
                    </b-row>
                    <b-row>
                      <b-col>
                        <div>
                          <!-- WELL SELECTOR -->
                          <b-form-select
                            v-model="pozzo_temp.selected"
                            :options="pozzo_temp.options"></b-form-select>
                        </div>
                      </b-col>
                    </b-row>
                  </b-card>
                </b-tab>
                <b-tab>
                  <template v-slot:title>
                    <b-icon icon="bar-chart-steps"></b-icon>
                    LITOSTRATIGRAFIA
                  </template>
                  <b-card id="popoverLito" bg-variant="light" class="mt-1">
                    <b-row class="mb-5">
                      <b-col>
                        <div style="width: 100%">
                          <!-- SINGLE BAR PLOT FOR LITHOLOGY -->
                          <barlito :aggregation_bar="bar.litologia"/>
                        </div>
                      </b-col>
                    </b-row>
                    <b-row>
                      <b-col>
                        <div>
                          <!-- WELL SELECTOR -->
                          <b-form-select
                            v-model="pozzo_lito.selected"
                            :options="pozzo_lito.options"></b-form-select>
                        </div>
                      </b-col>
                    </b-row>
                  </b-card>
                </b-tab>
              </b-tabs>
            </b-card>
          </b-col>
          <b-col cols="12">
            <b-card class="my-4" style="height: 760px">
              <b-tabs content-class="mt-3" justified pills card>
                <b-tab active>
                  <template v-slot:title>
                    <b-icon icon="bezier2"></b-icon>
                    ALLUVIAL DIAGRAM
                  </template>
                  <b-card id="popoverParallel" bg-variant="light" style="height: 600px">
                    <parallel :datiparallel="datiparallel"></parallel>
                  </b-card>
                </b-tab>
                <b-tab>
                  <template v-slot:title>
                    <b-icon icon="pie-chart"></b-icon>
                    SUNBURST
                  </template>
                  <b-card id="popoverSunburst" bg-variant="light" style="height:600px">
                    <sunburst></sunburst>
                  </b-card>
                </b-tab>
              </b-tabs>
            </b-card>
          </b-col>
        </b-row>
      </div>
    </b-container>
  </b-overlay>
</template>
<script>
import * as d3 from 'd3';
import crossfilter from 'crossfilter';
import chart from './chart';
import mappa from './mappa';
import scattertemp from './scattertemp';
import barlito from './barlito';
import sunburst from './sunburst';
import parallel from './parallel';

// CROSSFILTER DATA MANAGEMENT
// CROSSFILTER INSTANCES
let cf;
let cfTemp;
let cfLito;

// CROSSFILTER DIMENSIONS
let dregione;
let dtnome;
let dlitonome;
export default {
  name: 'dashboard',
  components: {
    scattertemp,
    chart,
    mappa,
    barlito,
    sunburst,
    parallel,
  },
  data() {
    return {
      show: true,
      nascondi: false,
      // DATASETS ARRAY
      reports: [],
      reports_temp: [],
      reports_litstr: [],

      // DATASETS FILTERED FOR TEMPERATURE AND LITHOLOGY CHART
      filtro_temperature: [],
      filtro_litologia: [],

      // START REGION SELECTOR DATA
      regione: {
        selected: 'TUTTA L\'ITALIA',
        options: [],
      },
      // END REGION SELECTOR DATA

      // START SLIDERPROF DATA
      sliderprof: {
        valore: 0,
        max: 0,
      },
      // END SLIDERPROF DATA

      // START SLIDERQUOTA DATA
      sliderquota: {
        valore: 0,
        min: 0,
        max: 0,
      },
      // END SLIDERQUOTA DATA

      // START TABLE DATA
      tabella: {
        selectMode: 'multi',
        rowSelected: [],
        fields: [
          {
            key: 'key',
            label: 'ID',
            sortable: true,
          },
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
            key: 'scopo',
            label: 'Scopo',
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
      // END TABLE DATA

      // START FILTER TABLE DATA
      filters: {
        key: '',
        nome: '',
        prof: '',
        quota: '',
        tipo: '',
        scopo: '',
        uso: '',
        esito: '',
        stato: '',
      },
      // END FILTER TABLE DATA

      // START MAP LABELS SELECTOR DATA
      selettore: {
        selected: 'NESSUN POZZO SELEZIONATO',
        options: Array,
      },
      // END MAP LABELS SELECTOR DATA

      // START MAP DATA
      datimappa: [],
      chart: {
        profalt: [],
      },
      // END MAP DATA

      // START SELECTOR TEMPERATURE DATA
      pozzo_temp: {
        selected: 'NESSUN POZZO SELEZIONATO',
        options: Array,
      },
      // END SELECTOR TEMPERATURE DATA

      // START SELECTOR LITO DATA
      pozzo_lito: {
        selected: 'NESSUN POZZO SELEZIONATO',
        options: Array,
      },
      // END SELECTOR LITHO DATA

      // START SCATTER TEMP DATA
      scatter: {
        temperature: [],
      },
      // END SCATTER TEMP DATA

      // START SINGLE BAR LITHO DATA
      bar: {
        litologia: [],
      },
      // END SINGLE BAR LITHO DATA

      // START PARALLEL COORDINATE DATA
      datiparallel: [],
      // END PARALLEL COORDINATE DATA
    };
  },
  created() {
    // eslint-disable-next-line no-return-assign
    setTimeout(() => this.show = false, 25 * 1000);
  },
  mounted() {
    // POZZI.JSON LOADING
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

        // CROSSFILTER GROUPING AND FILTERING
        cf = crossfilter(this.reports);
        dregione = cf.dimension(d => d.regione);
        this.regione.options = ['TUTTA L\'ITALIA'].concat(dregione.group()
          .reduceCount()
          .all()
          .map(v => v.key));
        this.regione.selected = this.regione.options[0];
        this.selettore.options = ['PROFONDITÀ | QUOTA', 'LOCALIZZAZIONE', 'PROPRIETARIO', 'CONDIZIONI'];
        this.selettore.selected = this.selettore.options[0];

        // D3 DATA MANIPULATION
        this.sliderprof.valore = d3.max(this.reports, d => d.prof);
        this.sliderquota.valore = d3.max(this.reports, d => d.quota);
        this.sliderprof.max = d3.max(this.reports, d => d.prof);
        this.sliderquota.max = d3.max(this.reports, d => d.quota);
        this.sliderquota.min = d3.min(this.reports, d => d.quota);

        this.refreshTable();
      });

    // POZZI_TEMP.JSON LOADING
    fetch('static/data/pozzi_temp.json')
      .then(res => res.json())
      .then((out) => {
        this.reports_temp = out.map(d => ({
          tprof: +d.prof,
          ttemp: +d.temp,
          tkey: +d.key,
          tnome: d.nome,
        }));

        // CROSSFILTER GROUPING AND FILTERING
        cfTemp = crossfilter(this.reports_temp);
        dtnome = cfTemp.dimension(d => d.tnome);
        this.filtro_temperature = dtnome.filter(this.pozzo_temp.selected);
      });

    // POZZI_LITSTR.JSON LOADING
    fetch('static/data/pozzi_litstr.json')
      .then(res => res.json())
      .then((out) => {
        this.reports_litstr = out.map(d => ({
          prof: +d.aprof,
          lito: d.litologia,
          nomepozzo: d.nome,
        }));

        // CROSSFILTER GROUPING AND FILTERING
        cfLito = crossfilter(this.reports_litstr);
        dlitonome = cfLito.dimension(d => d.nomepozzo);
        this.filtro_litologia = dlitonome.filter(this.pozzo_lito.selected);
      });
  },

  computed: {
    // FILTERED FUNCTION FOR TABLE COLUMN
    filtered() {
      const filtered = this.tabella.rowSelected.filter(item => Object.keys(this.filters)
        .every(key =>
          String(item[key])
            .includes(this.filters[key])));
      return filtered.length > 0 ? filtered : [{
        key: '',
        nome: '',
        prof: '',
        quota: '',
        tipo: '',
        scopo: '',
        uso: '',
        esito: '',
        stato: '',
      }];
    },

    // COMPUTING NUMBER OF RECORD FILTERED
    numberofrecords() {
      if (this.tabella.rowSelected.length === 0) {
        return 0;
      }
      return this.filtered.length;
    },
  },

  methods: {
    onmapclick(value) {
      this.pozzo_temp.selected = value;
      this.pozzo_lito.selected = value;
    },
    onchartclick(value) {
      this.pozzo_temp.selected = value;
      this.pozzo_lito.selected = value;
    },

    // CROSSFILTERING REGIONE
    refreshTable() {
      if (this.regione.selected === 'TUTTA L\'ITALIA') {
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
      // ROW SELECTED IN THE TABLE
      this.selectedTable = items;
      this.filtro_temperature = this.reports_temp.filter(d => d.tnome === this.pozzo_temp.selected);

      // DATA USED FOR SCATTER TEMPERATURE COMPONENT
      this.scatter.temperature = this.filtro_temperature.map(v => ({
        nome: v.tnome,
        temp: +v.ttemp,
        prof: +v.tprof,
        text: `${+v.ttemp}°C`,
      }));
      this.filtro_litologia = this.reports_litstr.filter(d =>
        d.nomepozzo === this.pozzo_lito.selected);

      // DATA USED FOR LITHO COMPONENT
      this.bar.litologia = this.filtro_litologia.map(v => ({
        x: 0,
        y: +v.prof,
        name: v.lito,
        well: `pozzo: ${this.pozzo_lito.selected}`,
      }));

      // DATA USED FOR PROF ALT CHART COMPONENT
      this.chart.profalt = this.selectedTable.map(v => ({
        key: v.nome,
        value1: +v.prof,
        value2: +v.quota,
        text1: `${+v.prof * -1} metri`,
        text2: `${+v.quota} metri`,

      }));

      // DATA USED FOR MAP COMPONENT
      this.datimappa = this.selectedTable.map(v => ({
        lon: v.lon_wgs84,
        nome: v.nome,
        lat: v.lat_wgs84,
        tipo: v.tipo,
        prof: v.prof,
        geoinfo: [
          `Nome:${v.nome}`,
          `Latitudine:${v.lat_wgs84}`,
          `Longitudine:${v.lon_wgs84}`,
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
      // DATA USED FOR PARALLEL COORDINATE COMPONENT
      this.datiparallel = this.selectedTable.map(v => ({
        tipo: v.tipo,
        esito: v.esito,
        scopo: v.scopo,
        uso: v.uso,
        stato: v.stato,
      }));

      // COMPUTED NUMBER OF SELECTED WELL
      this.tabella.selezionati = this.selectedTable.length;
      this.nome_selettore = this.selectedTable.map(v => ({
        nome: v.nome,
      }));

      // CONTENT OF THE SELECTORS FOR LITHO AND TEMPERATURE
      this.pozzo_temp.options = ['NESSUN POZZO SELEZIONATO'].concat(this.nome_selettore.map(d => d.nome));
      this.pozzo_temp.selected = this.pozzo_temp.options[0];
      this.pozzo_lito.options = ['NESSUN POZZO SELEZIONATO'].concat(this.nome_selettore.map(d => d.nome));
      this.pozzo_lito.selected = this.pozzo_lito.options[0];
    },

    // FUNCTION FOR SELECT/CLEAR ALL ROWS IN THE TABLE
    selectAllRows() {
      this.$refs.selectableTable.selectAllRows();
    },
    clearSelected() {
      this.$refs.selectableTable.clearSelected();
    },
  },

  // UPDATING CHANGES IN THE APP
  watch: {
    pozzo_temp: {
      handler(newVal) {
        dlitonome.filter(newVal.selected);
        this.onRowSelected();
      },
      deep: true,
    },
    pozzo_lito: {
      handler(newVal) {
        dtnome.filter(newVal.selected);
        this.onRowSelected();
      },
      deep: true,
    },

    regione: {
      handler(newVal) {
        if (newVal.selected === 'TUTTA L\'ITALIA') {
          dregione.filter(null);
        } else {
          dregione.filter(newVal.selected);
        }
        this.refreshTable();
      },
      deep: true,
    },
    sliderprof: {
      handler(newVal, oldVal) {
        if (newVal !== oldVal) {
          this.sliderprof.valore = newVal;
        }
        this.refreshTable();
      },
      deep: true,
    },
    sliderquota: {
      handler(newVal, oldVal) {
        if (newVal !== oldVal) {
          this.sliderquota.valore = newVal;
        }
        this.refreshTable();
      },
      deep: true,
    },
  },
};
</script>

<style scoped>
#loading-wrapper {
  position: fixed;
  width: 100%;
  height: 100%;
  left: 0;
  top: 0;
}

#loading-text {
  display: block;
  position: absolute;
  top: 50%;
  left: 50%;
  color: rgb(54, 58, 63);
  width: 100px;
  height: 30px;
  margin: -7px 0 0 -45px;
  text-align: center;
  font-family: 'PT Sans Narrow', sans-serif;
  font-size: 20px;
}

#loading-content {
  display: block;
  position: relative;
  left: 50%;
  top: 50%;
  width: 170px;
  height: 170px;
  margin: -85px 0 0 -85px;
  border: 3px solid #F00;
}

#loading-content {
  border: 3px solid transparent;
  border-top-color: rgb(61, 132, 243);
  border-bottom-color: rgb(61, 132, 243);
  border-radius: 50%;
  -webkit-animation: loader 2s linear infinite;
  -moz-animation: loader 2s linear infinite;
  -o-animation: loader 2s linear infinite;
  animation: loader 2s linear infinite;
}

@keyframes loader {
  0% {
    -webkit-transform: rotate(0deg);
    -ms-transform: rotate(0deg);
    transform: rotate(0deg);
  }

  100% {
    -webkit-transform: rotate(360deg);
    -ms-transform: rotate(360deg);
    transform: rotate(360deg);
  }
}

.tabella {
  height: 400px;
  overflow: scroll;
  overflow-style: marquee-block;
}
</style>
