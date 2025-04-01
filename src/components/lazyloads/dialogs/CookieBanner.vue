<template>
  <div>
    <div
      id="slideSource"
      :class="[show_banner ? 'display-box ' : 'hide-box ']"
      :height="imageHeight"
      class="small-box box slideSource"
      :style="fade_done ? 'display: none' : ''"
    >
      <v-footer
        color="#FFEB3B"
        dark
        class="indigo lighten-1 white--text hid-box"
      >
        <!-- banner icon -->
        <v-col
          v-if="!$vuetify.breakpoint.mobile"
          class="col-lg-2 col-sm-2 col-md-2 col-xs-0 pa-1 ma-0"
          style="max-width: 180px"
        >
          <v-img
            max-width="150px"
            width="100%"
            :src="require('~/assets/images/icon.svg')"
          ></v-img>
        </v-col>
        <v-col
          :class="$vuetify.breakpoint.smAndDown ? ' ' : ''"
          style="min-width: 320px"
          class="pa-2 ma-2"
        >
          <!-- <v-card-text class="justify-left"> -->
          We use necessary cookies to make our site work. We’d also like to set
          other cookies to help us improve your experience with MyUSE. These
          will be set only if you accept. You can control the categories of
          cookies enabled by selecting the options within this banner. You can
          modify your cookie preferences at any time by clicking on the cookie
          settings button. Check out our
          <b>
            <nuxt-link color="black" :to="{ name: 'privacy' }">
              Privacy Notice
            </nuxt-link>
          </b>
          for more information.
          <!-- </v-card-text> -->
        </v-col>
        <!-- cookie buttons -->
        <v-col class="col-lg-3 col-md-3 col-sm-3 col-xs-12 py-0 ma-0">
          <!-- <v-btn
            id="confirmLoginButton"
            class="my-4"
            block
            @click="manage_coookies()"
          >
            Manage
          </v-btn> -->

          <v-btn
            id="confirmLoginButton"
            class="my-4"
            block
            @click="rejectAll()"
          >
            Reject
          </v-btn>

          <v-btn
            id="confirmLoginButton"
            class="my-4"
            block
            @click="acceptAll()"
          >
            Accept
          </v-btn>
        </v-col>
      </v-footer>
    </div>
    <!-- show the main cookie settings -->
    <!-- <div v-show="show_popup">
      <cookie-dialog ref="dialog" @close="closePopOut()"></cookie-dialog>
    </div> -->
  </div>
</template>
<script>
// import CookieDialog from './CookieDialog.vue'
import Cookies from 'js-cookie'
export default {
  name: 'CookieBanner',
  // components: { CookieDialog },
  data: () => ({
    code: null,
    show_banner: false,
    show_popup: false,
    show: true,
    allow_clear_banner: false,
    slideMode: false,
    fade_done: false,
  }),
  computed: {
    imageHeight() {
      switch (this.$vuetify.breakpoint.name) {
        case 'xs':
          return '200px'
        case 'sm':
          return '400px'
        case 'md':
          return '600px'
        case 'lg':
          return '800px'
        case 'xl':
          return '1000px'
      }
      return '0px'
    },
  },
  mounted() {
    // check if any cookies have been set
    const Cookie = Cookies.get('remember-me', { parseJSON: false })

    if (!Cookie) {
      this.slideToggle(true)
    } else {
      this.slideToggle(false)
      this.show_popup = true
      this.show_banner = false
    }
  },
  methods: {
    slideToggle(val) {
      setTimeout(() => {
        this.show_banner = val

        if (val === false) {
          setTimeout(() => {
            this.fadeOut()
          }, 200)
        }
      }, 200)
    },

    manage_coookies() {
      this.replacePopUp()
      this.$refs.dialog.openDialog()
    },
    acceptAll() {
      this.setCookies(1, 1)
    },
    rejectAll() {
      this.setCookies(0, 0)
    },
    closePopOut() {
      this.show_popup = false
    },
    replacePopUp() {
      this.show_popup = true
      this.slideToggle(false)
    },
    setCookies(session, analytics) {
      this.$store.dispatch('auth/setGAPreference', analytics)
      this.$store.dispatch('auth/setRememberMePreference', session)

      this.show_popup = true
      this.slideToggle(false)
    },
    fadeOut() {
      this.fade_done = true
    },
  },
}
</script>
<style scoped>
.box {
  width: 100%;
  overflow: hidden;
  bottom: 0px;
  position: fixed;
}
.hid-box {
  top: 100%;
  position: relative;
  transition: all 0.2s ease-out;
}
.display-box > .hid-box {
  top: 0px;
}

.hide-box > .hid-box {
  top: 432px;

  /* display: none; */
}
a {
  color: black;
  text-decoration: none;
}

#slideSource {
  opacity: 1;
  transition: opacity 1s;
}

#slideSource.fade {
  opacity: 0;
}
</style>
