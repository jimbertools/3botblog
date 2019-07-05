<template>
  <footer class="footer">
    <!-- <a href="https://prismic.io" target="_blank" rel="noopener">
      <img class="logo" src="../assets/img/prismic-logo.svg" alt="Prismic">
    </a>-->
    <v-container>
      <v-layout row wrap justify-center>
        <v-flex xs3>
          <a href="https://threefold.io" target="_blank" rel="noopener" class="text-xs-center">
            <img class="logo" src="../assets/img/3botlogo.png" alt="3Bot - Threefold" />
            <br />Powered by 3Bot
          </a>
        </v-flex>
      </v-layout>
    </v-container>
  </footer>
</template>

<script>
export default {
  name: "FooterPrismic"
};
</script>

<style scoped>
.footer {
  padding: 40px 0;
  max-width: 700px;
  margin: 0 auto;
  text-align: center;
  border-top: 1px solid #dadada;
}
.logo {
  height: 50px;
}
</style>
