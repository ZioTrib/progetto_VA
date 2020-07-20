<template>
  <b-row class="counter">
    <b-col>
      <h4 class="measure">{{measure}}</h4>
      <h2 class="value">{{value}}</h2>
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
