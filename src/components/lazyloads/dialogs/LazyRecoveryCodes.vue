<template>
  <v-dialog v-model="propModel" content-class="round" max-width="350px">
    <template v-slot:activator="{ on, attrs }">
      <v-spacer></v-spacer>
      <v-btn
        class="mx-3 align-right infoButton"
        fab
        color="primary"
        dark
        v-bind="attrs"
        x-small
        v-on="on"
      >
        <v-icon
          class="mx-4 infoButton"
          dark
          v-bind="attrs"
          color="secondary"
          v-on="on"
          >mdi-comment-question-outline</v-icon
        >
      </v-btn>
    </template>

    <v-card class="myuse-border pa-4">
      <!-- <v-card-title class="text-center headline"> Recovery Codes </v-card-title> -->
      <!-- header -->
      <v-card-text class="py-0">
        The following <b>recovery codes</b> can be used once to log in, if you
        have forgotten your password.
      </v-card-text>
      <!-- display codes -->
      <v-card-text class="mx-2 py-1">
        <v-list class="recovery-card">
          <v-list-item v-for="(code, i) in codes" :key="i" class="py-0">
            <v-list-item-icon>
              <v-icon color="pink"> mdi-star </v-icon>
            </v-list-item-icon>
            <v-list-item-content class="py-0">
              {{ code }}
            </v-list-item-content>
          </v-list-item>
        </v-list>
      </v-card-text>
      <v-card-text class="py-0 pb-2">
        Copy the following codes and store them somewhere safe. You will not be
        given these again.
      </v-card-text>
      <!-- dialog buttons -->
      <v-card-actions class="pa-3 text-center justify-center">
        <v-btn color="primary" @click="downloadCodes()"> Save Codes </v-btn>
        <v-btn color="primary" @click="exit()"> OK! </v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>
<script>
export default {
  name: 'RecoveryCodes',
  props: {
    codes: {
      type: Array,
      required: true,
    },
    dialog: {
      type: Boolean,
      required: true,
    },
  },
  computed: {
    propModel: {
      get() {
        return this.dialog
      },
      set(value) {
        this.$emit('input', value)
      },
    },
  },

  methods: {
    // exit modal window
    exit() {
      this.$emit('closeRecoveryDialog', false)
    },
    // download the recovery codes
    downloadCodes() {
      const link = document.createElement('a')
      link.href = window.URL.createObjectURL(
        new Blob([JSON.stringify(this.codes)])
      )
      link.setAttribute('download', 'MyUSE 2FA Recovery Codes.txt')
      document.body.appendChild(link)
      link.click()
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
  overflow: hidden;
}
</style>
