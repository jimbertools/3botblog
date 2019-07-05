<template>
  <v-app>
  <div id="app">
    <router-view/>
    <footer-prismic/>
  </div>
  </v-app>
</template>

<script>
import FooterPrismic from './components/FooterPrismic.vue'

export default {
  name: 'App',
  components: {
    FooterPrismic
  }
}
</script>

<style>
@import "./assets/css/resetr.css";
@import "./assets/css/common.css";
@import "./assets/css/common.css";
</style>