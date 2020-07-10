<template>
  <b-container class="dashboard">
    <b-row>
      <b-col cols="9">
        <h5>Mappa dei pozzi in italia</h5>
        <div style="height:500px; background-color: beige"></div>
      </b-col>
      <b-col cols="3">
        <h5>informazioni del pozzo</h5>
        <div style="height:250px; background-color: beige"></div>
        <b-col>
          <div>
            <h5>Attributi</h5>
            <b-form-select v-model="selected" :options="options"></b-form-select>
            <div class="mt-3">Selected: <strong>{{ selected }}</strong></div>
          </div>
        </b-col>
      </b-col>
    </b-row>

    <b-row>
      <b-col>
        <h5> quota e profondità pozzi </h5>
        <div style="height:400px; background-color: beige"></div>
      </b-col>
    </b-row>
    <b-row>
      <b-col>
        <h5> 1</h5>
        <div style="height:250px; background-color: beige"></div>
      </b-col>
      <b-col>
        <h5> 2 </h5>
        <div style="height:250px; background-color: beige"></div>
      </b-col>
      <b-col>
        <h5> 3 </h5>
        <div style="height:250px; background-color: beige"></div>
      </b-col>
    </b-row>
  </b-container>
</template>


<script>


export default {
  name: 'dashboard',
  components: {
  },
  data() {
    return {
      year: {
        value: 2001,
        options: [2001, 2002, 2003],
      },
      boatType: {
        value: 'All',
        options: ['All', 'Rustic', 'Raft'],
      },
      reports: [],
    };
  },

  mounted() {
    fetch('static/data/pozzi.json')
      .then(res => res.json())
      .then((out) => {
        this.reports = out.map(d => ({
          prof: +d.prof,
          key: +d.key,
        }));
      });
  },
};
</script>

<style scoped>

</style>
