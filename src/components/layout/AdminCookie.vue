<template>
  <div v-if="isOpen" class="cookie">
    <v-alert prominent type="info" color="#6859de">
      <v-row>
        <v-col class="grow">
          <slot name="message">
            We use cookies to make our application to work. To find out more
            about our use of cookies, please see our Privacy Policy. By
            continuing to browse our website, you agree to our use of cookies.
          </slot>
        </v-col>
        <v-col class="shrink d-flex justify-end" cols="8" md="3">
          <v-btn class="ma-2" @click="accept">{{ buttonTextAccept }}</v-btn>
        </v-col>
      </v-row>
    </v-alert>
  </div>
</template>

<script>
export default {
  name: 'AdminCookieMessage',
  props: {
    buttonTextAccept: {
      type: String,
      default: 'Accept',
    },
    buttonTextDeny: {
      type: String,
      default: 'Deny',
    },
    message: {
      type: String,
      default:
        'We use cookies to provide our services and for analytics and marketing. To find out more about our use of cookies, please see our Privacy Policy. By continuing to browse our website, you agree to our use of cookies.',
    },
    position: {
      type: String,
      default: 'top',
    },
  },
  data() {
    return {
      isOpen: false,
    }
  },
  computed: {
    containerPosition() {
      return `cookie--${this.position}`
    },
  },

  methods: {
    accept() {
      if (process.browser) {
        this.isOpen = false
        localStorage.setItem('GDPR:accepted', true)
      }
    },
  },
}
</script>

<style lang="sass" scoped>
.cookie
  z-index: 1
  position: fixed
  bottom: 0
  &__link
    color: #ffffff
    text-decoration: underline
    transition: all .25s
    &:hover
      text-decoration: none
</style>
