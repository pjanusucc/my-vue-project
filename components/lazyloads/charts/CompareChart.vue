<template>
  <div v-if="scores" class="compare-chart-borders">
    <div class="compare-chart-wrapper">
      <div class="compare-bar-wrapper">
        <div
          class="compare-bar"
          :style="{
            height: getHeight(scores[0]) + 'px',
          }"
        ></div>
        <p class="compare-bar-score" :class="'bar-size-' + getStyle(scores[0])">
          {{ scores[0] }}<span>{{ labels.metric }}</span>
        </p>
        <div class="compare-bar-label">
          {{ labels.first }}
        </div>
      </div>
      <div class="compare-bar-wrapper">
        <div
          class="compare-bar"
          :style="{
            height: getHeight(scores[1]) + 'px',
            backgroundColor: color,
          }"
        ></div>
        <p class="compare-bar-score" :class="'bar-size-' + getStyle(scores[1])">
          {{ scores[1] }}<span>{{ labels.metric }}</span>
        </p>
        <div class="compare-bar-label">{{ labels.second }}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CompareChart',
  props: {
    set: {
      type: Object,
      required: true,
    },
    labels: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      colors: ['var(--v-success-base)', 'var(--v-gray-base)'],
      high_value: 0,
      low_value: 0,
      min_height: 30,
      mid_height: 60,
      max_height: 150,
    }
  },
  computed: {
    scores() {
      return this.set.scores || [500000, 500000]
    },
    scale() {
      if (this.scores[0] >= this.scores[1]) {
        return this.max_height / this.scores[0]
      } else {
        return this.max_height / this.scores[1]
      }
    },
    color() {
      if (this.checkAllZero()) {
        return this.colors[1]
      }
      return this.set.status === 1 ? this.colors[0] : this.colors[1]
    },
  },
  mounted() {
    if (this.set.length > 0) {
      this.high_value = Math.max(this.set.scores[0], this.set.scores[1])
      this.low_value = Math.min(this.set.scores[0], this.set.scores[1])
    } else {
      this.high_value = Math.max(3, 1)
      this.low_value = Math.min(1, 3)
    }
  },
  methods: {
    getHeight(score) {
      let height = this.scale * score
      if (this.checkAllZero() || height < this.min_height) {
        height = this.min_height
      }
      return height
    },
    getStyle(score) {
      const height = this.getHeight(score)
      if (height === this.min_height) {
        return 'min'
      }
      if (height < this.mid_height) {
        return 'medium'
      }
      if ((score + '').length > 2) {
        return 'max'
      }
      return 'normal'
    },
    checkAllZero() {
      return this.scores.every((item) => item === 0)
    },
  },
}
</script>
