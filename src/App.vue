<template>
  <div id="app">
    <List />
  </div>
</template>

<script>
import List from './components/List.vue';

export default {
  name: 'app',
  components: {
    List,
  },
};
</script>

<style lang="scss">

  @import './globals/_variables.scss';

  #app {
    min-height: 100vh;
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: $darkBlue;
  }
</style>
