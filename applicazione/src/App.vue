<template>
  <div id="app" class="bg-primary">
    <dashboard></dashboard>
  </div>
</template>

<script>
import dashboard from './components/dashboard';

export default {
  name: 'App',
  components: {
    dashboard,
  },
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
