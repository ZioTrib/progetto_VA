<template>
  <b-row class="counter">
    <b-col>
      <h3 class="measure">{{measure}}</h3>
      <h1 class="value">{{value}}</h1>
    </b-col>
  </b-row>
</template>

<script>
export default {
  name: 'informazioni',
  props: {
    measure: String,
    value: Number,
  },
};
</script>

<style scoped>

</style>
