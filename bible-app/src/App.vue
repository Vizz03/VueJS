<template>
  <div id="app">
    <img alt="Shokologo" src="./assets/bible.svg" style="max-width:100%;height:auto;">
    <Bible/>
  </div>
</template>

<script>
import Bible from './components/Bible.vue'

export default {
  name: 'App',
  components: {
    Bible
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
