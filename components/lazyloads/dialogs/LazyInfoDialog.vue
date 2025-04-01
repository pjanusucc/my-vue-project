<template>
  <v-row no-gutters persistent style="margin-top: 0px; height: 0px">
    <v-spacer style="margin-top: 0px; height: 0px"> </v-spacer>
    <v-dialog v-model="dialog" persistent max-width="400">
      <template v-slot:activator="{ on, attrs }">
        <v-icon
          class="mx-4 infoButton"
          fab
          dark
          :class="{ leftButton: left }"
          v-bind="attrs"
          color="primary"
          v-on="on"
          >mdi-comment-question-outline</v-icon
        >
      </template>
      <v-card>
        <v-card-title class="headline"> What is this? </v-card-title>
        <v-card-text v-html="$md.render(info)"></v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary darken-1" text @click="dialog = false">
            OK, let me continue!
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
</template>
<script>
export default {
  name: 'InfoDialog',
  props: {
    info: {
      type: String,
      required: true,
    },
    left: {
      type: Boolean,
      required: false,
    },
  },
  data() {
    return {
      dialog: false,
    }
  },
}
</script>
<style scoped>
.infoButton {
  bottom: 20px;
}
.leftButton {
  bottom: 42px;
  left: -28px;
}
</style>
