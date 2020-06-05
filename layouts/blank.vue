<template>
  <v-app centered>
    <v-content>
      <v-container class="v-container-full-width">
        <nuxt />
      </v-container>
    </v-content>
    <v-footer :fixed="fixed" app>
      <span>Powered by SunnyCal &copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script></script>
<style>
.v-application {
  background-color: white !important;
}
</style>
