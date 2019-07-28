<template>
  <div id="app">
    <!-- img alt="Vue logo" src="./assets/logo.png" -->
    <Assbutt
        title="Flight of the Assbutt"
        subtitle="Team Bangarang 2019"
    />
  </div>
</template>

<script>
import Assbutt from './components/Assbutt.vue'

export default {
  name: 'app',
  components: {
    Assbutt
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
