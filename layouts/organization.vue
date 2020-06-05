<template>
  <v-app light centered>
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

<script>
export default {
  data() {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'mdi-apps',
          title: 'Welcome',
          to: '/'
        },
        {
          icon: 'mdi-chart-bubble',
          title: 'Inspire',
          to: '/inspire'
        }
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'SunnyCal'
    }
  }
}
</script>
<style scoped lang="scss">
.v-application {
  background-color: $background-color !important;
  font-family: $body-font-family !important;
}
</style>
