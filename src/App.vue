<template>
  <v-app>
    <layout-admin>
      <Algorithm />
    </layout-admin>
  </v-app>
</template>

<script>
import layoutAdmin from '@/layouts/layout-admin.vue'
import Algorithm from '@/pages/algorithm.vue'

export default {
  name: 'App',
  components: {
    layoutAdmin,
    Algorithm,
  },
}
</script>
