<template>
  <span
    class="scroll-anchor"
    :class="['scroll-anchor--type-' + type, 'scroll-anchor--value-' + value]"
    :style="{ top: offset }"
  ></span>
</template>

<script>
import mitt from 'mitt'
const eventBus = mitt()
export { eventBus }

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
  methods: {
    getValue() {
      switch (this.type) {
        case 'anchor':
          return new URL(window.location.href).hash.substring(1)
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
    eventBus.on('scroll-anchor:update', hash => {
      if (!hash) return
      return this.check(hash.charAt(0) === '#' ? hash.substring(1) : hash)
    })
  },
}
</script>

<style lang="scss">
.scroll-anchor {
  display: block;
  position: relative;
  pointer-events: none;
}
</style>
