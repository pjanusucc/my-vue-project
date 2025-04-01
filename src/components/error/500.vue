<template>
  <div class="error-404">
    <h1>Oh no! There's been an error.</h1>
    <h2>{{ error.message }}</h2>
    <p>Please navigate back and try again.</p>
    <v-flex>
      <v-btn large color="primary" @click="back"> Back </v-btn>
    </v-flex>
  </div>
</template>
<script>
export default {
  name: 'Error500',
  props: {
    error: {
      type: Object,
      default: () => {},
    },
  },
  methods: {
    back() {
      this.$router.back()
    },
  },
}
</script>
