<template>
  <v-app id="app">
    <v-main>
      <v-container class="fill-height" fluid>
        <nuxt />
      </v-container>
      <cookie-banner></cookie-banner>
    </v-main>
  </v-app>
</template>
<script>
import CookieBanner from '../components/lazyloads/dialogs/CookieBanner.vue'
export default {
  components: {
    CookieBanner,
  },
  transition: 'fade',
  created() {
    this.$vuetify.theme.dark = true
  },
}
</script>
<!-- <style scoped>
.fade-enter-active {
  transition: opacity 2.8s;
}

.fade-leave-active {
  transition: opacity 2.8s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style> -->
