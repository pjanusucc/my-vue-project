<template>
  <v-dialog v-model="open" content-class="round" max-width="350">
    <template v-slot:activator="{ on, attrs }">
      <v-spacer></v-spacer>
      <v-btn
        class="mx-3 align-right infoButton"
        fab
        dark
        color="primary"
        v-bind="attrs"
        x-small
        v-on="on"
      >
        <v-icon
          class="mx-4 infoButton"
          dark
          v-bind="attrs"
          color="white"
          v-on="on"
          >mdi-comment-question-outline</v-icon
        >
      </v-btn>
    </template>
    <v-card class="myuse-border pa-5">
      <v-card-title class="headline"> Forgot your password? </v-card-title>
      <v-card-text
        >Enter your email address to submit a forgot password request.
        <v-text-field
          v-model="email"
          placeholder="Enter your email"
          prepend-icon="mdi-email"
          data-vv-name="Email"
          label="Email"
          full-width
          name="email"
          required
        ></v-text-field>
      </v-card-text>
      <!-- dialog buttons -->
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn
          color="primary"
          @click="
            sendForgotPasswordEmail()
            open = false
          "
        >
          Submit
        </v-btn>
        <v-btn color="primary" @click="open = false"> Exit </v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>
<script>
const api = process.env.BASEAPI || 'https://api.myuse.ie/api' // temp fix to circumvent netlify env var issuess

export default {
  name: 'ForgotPassword',
  props: {
    // dialog: {
    //   type: Boolean,
    //   required: true,
    // },
    // email: {
    //   type: String,
    //   required: true,
    // },
  },
  data: () => ({
    selected: [],
    open: false,
    answered: [],
    email: null,
  }),
  mounted() {
    //  this.open = this.dialog
  },
  methods: {
    // send this user a reset password email
    sendForgotPasswordEmail() {
      const self = this
      this.$axios
        .post(api + '/auth/password_reset_email', {
          email: this.email,
        })
        .then((resp) => {
          self.$dialog.message.error(
            'An email will been sent to your address if you have an account with us.',
            {
              position: 'top',
              color: 'red',
              timeout: 3000,
            }
          )
        })
        .catch((err) => {
          self.$dialog.message.error(
            'Error sending request to user: ' +
              JSON.stringify(err.response.data),
            {
              position: 'top',
              color: 'red',
              timeout: 3000,
            }
          )
        })
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
