<template>
  <div id="app">
    <ShortTermMemory />
  </div>
</template>

<script>
import ShortTermMemory from './components/ShortTermMemory.vue';
export default {
  name: 'App',
  components: { ShortTermMemory },
};
</script>

<style lang="scss">
body {
  margin: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  color: #2c3e50;
}
</style>
