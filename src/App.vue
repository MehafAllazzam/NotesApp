<template>
  <div id="app">
    <NotesApp></NotesApp>
  </div>
  
</template>

<script>
import NotesApp from './components/NotesApp.vue'
/* import the fontawesome core */

export default {
  name: 'App',
  components: {
    NotesApp
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
