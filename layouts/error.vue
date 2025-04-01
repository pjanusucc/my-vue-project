<template>
  <transition name="fade">
    <v-row>
      <v-col cols="12">
        <v-card
          :class="{ 'elevation-2': !$vuetify.breakpoint.mobile }"
          class="rounded-xl myuse-border round"
        >
          <v-img
            :src="require('~/assets/images/' + errorText.image)"
            class="embed-image"
            aspect-ratio="1.6"
          />

          <v-card-text>
            <v-row justify="center">
              <v-col cols="12" sm="10" xs="12">
                <h2 class="blockquote text-center mb-10">
                  {{ errorText.body }}
                </h2>
                <v-alert v-if="debug" type="info" outlined class="my-10">
                  {{ error.message }}
                </v-alert>

                <div class="text-center my-5">
                  <v-btn large color="primary" @click="returnMenu">
                    {{ backBtn }}
                  </v-btn>
                </div>
              </v-col>
            </v-row>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </transition>
</template>

<script>
import error500 from '~/components/error/500.vue'
export default {
  name: 'Error',
  layout: 'layout-admin',
  props: {
    error: {
      type: Object,
      required: true,
    },
  },
  data: () => ({
    debug: false,
    title: 'MyUSE',
    backBtn: 'Go Back',
    homeBtn: 'Home Page',
    error404: {
      image: 'lost.svg',
      title: 'Page Not Found',
      body: `Oh no! This page can't be found. Please try going back or to our homepage.`,
    },
    error500: {
      image: 'error.svg',
      title: 'Page Error',
      body: `Oh no! There's been an error. Please try reloading the page or going back.`,
    },
  }),
  computed: {
    errorPage() {
      return error500
    },
    errorText() {
      return this.error500
    },
  },
  methods: {
    back() {
      this.$router.push('/')
    },
    returnMenu() {
      this.$router.push('/')
    },
  },
}
</script>
<style scoped>
.round {
  border-radius: 60px;
  overflow-y: hidden;
}

.myuse-border {
  border-color: white;
  border-style: solid;
  border-radius: 60px;
}
</style>
