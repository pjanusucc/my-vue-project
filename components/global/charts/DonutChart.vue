<template>
  <div class="chart-wrapper">
    <!-- display data on gauge -->
    <VueSvgGauge
      :start-angle="0"
      :end-angle="360"
      :value="score"
      :min="0"
      :max="scale"
      :gauge-color="colors"
      :separator-step="0"
      :scale-interval="0"
      :inner-radius="60"
    >
      <div class="score">
        {{ score }}<span>{{ unit }}</span>
      </div>
    </VueSvgGauge>

    <div v-if="type == 'QuizScore'" class="chart-wrapper gauge-chart-wrapper">
      <div class="risk">Your Quiz Score</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'DonutChart',

  props: {
    type: {
      type: String,
      default: '',
    },
    value: {
      type: Number,
      default: 0,
    },
    unit: {
      type: String,
      default: '/10',
    },
    scale: {
      type: Number,
      default: 10,
    },
  },
  computed: {
    score() {
      return this.value > 0
        ? this.value
        : this.$store.getters['profile/get' + this.type]
    },
    colors() {
      return [{ offset: 0, color: 'var(--v-primary-lighten2)' }]
    },
  },
}
</script>
<style scoped>
.chart-wrapper {
  box-shadow: 0px 0px;
}
</style>
