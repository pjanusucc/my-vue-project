<template>
  <v-dialog v-model="propModel" persistent max-width="430">
    <v-card justify="center">
      <!-- display page -->
      <v-card-title class="headline">
        {{ info.title }}
      </v-card-title>
      <div v-if="info.id === 'logout'" justify="center">
        <v-card-text>{{ info.body }}</v-card-text>
        <v-card-actions v-if="password">
          <v-spacer></v-spacer>
          <!-- copy code button -->
          <v-btn
            id="copyButton"
            v-model="password"
            v-clipboard:copy="password"
            large
            :color="copied ? 'primary' : 'secondary'"
            :class="copied ? 'white--text' : 'black--text'"
            class="copy-button"
            @click="copyCode()"
          >
            <span v-if="copied"> Copied <v-icon right>mdi-check</v-icon> </span>
            <span v-else>
              {{ password }}
              <v-icon right>mdi-content-copy</v-icon>
            </span>
          </v-btn>
          <v-spacer></v-spacer>
        </v-card-actions>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn large color="primary" @click="cancel()"> Cancel </v-btn>
          <v-btn color="primary" large @click="exit()"> Continue </v-btn>
        </v-card-actions>
      </div>
      <div v-else>
        <v-card-text>{{ info.body }}</v-card-text>
        <v-row
          class="justify-content: flex-end pa-3 mx-3 display: flex; justify-content: space-around;"
        >
          <v-col
            v-for="modu in info.mapped"
            :key="modu"
            :v-bind="modu"
            :data="modu"
            class="text-center mx-auto"
          >
            <v-chip label large color="success" outlined class="text-center">
              <strong>{{ modu }}</strong>
            </v-chip>
          </v-col>
        </v-row>
        <!-- bottom buttons -->
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            v-if="info.id === 'optional'"
            large
            color="primary"
            @click="goToOptionalModule('/dashboard')"
          >
            Menu
          </v-btn>
          <v-btn v-else large color="primary" @click="cancel()"> Cancel </v-btn>
          <v-btn color="primary" large @click="exit()"> Continue </v-btn>
        </v-card-actions>
      </div>
    </v-card>
  </v-dialog>
</template>
<script>
export default {
  name: 'NotifierDialog',
  props: {
    dialog: {
      type: Boolean,
      required: true,
    },
  },
  data: () => ({
    copied: false,
  }),
  computed: {
    propModel: {
      get() {
        return this.dialog
      },
      set(value) {
        this.$emit('input', value)
      },
    },
    // info to display in dialog
    info() {
      return this.$store.getters['navigation/getNotifierInfo']
    },
    content() {
      return this.$store.state.content.copycode
    },
    password() {
      return this.$store.getters['auth/getUserPassword']
    },
  },
  mounted() {
    this.storeHistory()
  },

  methods: {
    async storeHistory() {
      await this.$store.dispatch('navigation/storeNavigationHistory')
    },
    // exit modal window
    exit() {
      this.$store.commit('navigation/setDisplayNotification', false)
      if (this.info.id === 'logout' && this.password) {
        this.$store.dispatch('auth/logoutUser')
        this.$toast.success('Logged Out User')

        setTimeout(() => {
          this.$dialog.message.success('Logged Out User', {
            position: 'top',
            color: 'green',
            timeout: 3000,
          })
        }, 800)
      }
    },
    cancel() {
      this.$store.commit('navigation/setDisplayNotification', false)
    },
    // copy code to clipboard
    copyCode() {
      this.$copyText(this.password, this.$refs.container)
      this.copied = true
      setTimeout(() => (this.copied = false), 2000)
    },
    // navigate away from main course
    goToOptionalModule(url) {
      this.$store.dispatch('navigation/setOptionalModStatus', true)
      this.$store.commit('navigation/setDisplayNotification', false)
      this.$router.push(url)
    },
  },
}
</script>
