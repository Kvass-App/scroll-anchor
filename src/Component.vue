<template>
  <span
    class="scroll-anchor"
    :class="['scroll-anchor--type-' + type, 'scroll-anchor--value-' + value]"
    :style="{ top: offset }"
  ></span>
</template>

<script>
export default {
  props: {
    value: {
      type: String,
    },
    type: {
      type: String,
      enum: ['anchor', 'path'],
      default: 'anchor',
    },
    offset: String,
    delay: {
      type: Number,
      default: 0,
    },
    scrollOptions: {
      type: Object,
      default: () => ({ behavior: 'smooth' }),
    },
  },
  watch: {
    value: 'check',
    type: 'check',
    $route: {
      handler: 'check',
      deep: true,
    },
  },
  methods: {
    getValue() {
      switch (this.type) {
        case 'anchor':
          return window.location.hash.substring(1)
        case 'path':
          return window.location.pathname
      }
    },
    check() {
      if (this.getValue() !== this.value) return
      setTimeout(() => this.$el.scrollIntoView(this.scrollOptions), this.delay)
    },
  },
  mounted() {
    this.check()
  },
}
</script>

<style lang="scss">
.scroll-anchor {
  display: inline-block;
  position: relative;
  pointer-events: none;
}
</style>
