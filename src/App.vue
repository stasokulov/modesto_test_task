<template>
  <div id="app">
    <MainPage/>
  </div>
</template>

<script>
import MainPage from './views/MainPage.vue'

export default {
  name: 'App',
  components: {
    MainPage
  }
}
</script>

<style>
#app {
  font-family: 'Roboto Mono', sans-serif;
}

body {
  margin: 0;
  color: #000000;
}
</style>
