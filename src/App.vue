<template>
  <div id="app">
    <div class="jumbotron">
      <h1>Formulario</h1>
      <Navbar />
      <router-view></router-view> 
      <br>
    </div>
  </div>
</template>

<script>
import Navbar from './componentes/Navbar.vue'

export default {
  name: 'App',
  components: {
    Navbar
  }
}
</script>

<style>
#app {
  
}
.jumbotron {
  padding: 32px !important;
  }
</style>
