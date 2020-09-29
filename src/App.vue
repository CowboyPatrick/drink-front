<template>
<div id="app">
  <drinks/>
</div>
</template>

<script>
import drinks from './components/drinks'

export default {
  name: 'App',
  components: {
    drinks
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
