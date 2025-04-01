<template>
  <div
    class="chart-wrapper gauge-chart-wrapper"
    :class="'guage-score-' + score"
  >
    <VueSvgGauge
      :start-angle="-110"
      :end-angle="110"
      :value="score"
      :min="0"
      :max="10"
      :gauge-color="colors"
      :separator-step="0"
      :scale-interval="1"
      :inner-radius="60"
    >
      <div class="score">{{ score }}<span>/10</span></div>
    </VueSvgGauge>
    <div class="risk">{{ type }} risk</div>
  </div>
</template>

<script>
export default {
  name: 'GaugeChart',

  props: {
    info: {
      type: String,
      required: true,
    },
  },
  computed: {
    score() {
      return this.$store.getters['profile/getDASTScore']
    },
    type() {
      return this.$store.getters['profile/getUserType']
    },
    colors() {
      return [
        { offset: 0, color: 'var(--v-secondary-base)' }, // Low
        { offset: 30, color: 'var(--v-warning-base)' }, // Moderate
        { offset: 60, color: 'var(--v-error-lighten1)' }, // Substantial
        { offset: 90, color: 'var(--v-error-darken1)' }, // Severe
      ]
    },
  },
}
</script>
<style scoped>
/* chart-wrapper should almost always never have a box shadow  */
.chart-wrapper {
  box-shadow: 0px 0px;
}
</style>
