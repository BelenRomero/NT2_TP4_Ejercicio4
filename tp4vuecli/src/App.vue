<template>
  <div id="app">

    <div class="jumbotron">
      <h1>NT2 TP4 VueCli</h1>
      <br>
      <Navbar />
        <router-view></router-view>
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
</style>
