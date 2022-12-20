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
  // watch: {
  //   value: 'check',
  //   type: 'check',
  //   $route: {
  //     handler: 'check',
  //     deep: true,
  //   },
  // },
  methods: {
    getValue(event) {
      switch (this.type) {
        case 'anchor':
          return event
        case 'path':
          return window.location.pathname
      }
    },
    // getValueMounted() {
    //   switch (this.type) {
    //     case 'anchor':
    //       return new URL(window.location.href).hash.substring(1)
    //     case 'path':
    //       return window.location.pathname
    //   }
    // },

    // checkMounted() {
    //   if (this.getValueMounted() !== this.value) return

    //   setTimeout(() => this.$el.scrollIntoView(this.scrollOptions), this.delay)
    // },
    check(event) {
      if (this.getValue(event) !== this.value) return

      console.log(this.$el)
      setTimeout(() => this.$el.scrollIntoView(this.scrollOptions), this.delay)

      // this.$nextTick(() =>
      //   setTimeout(() => this.$el.scrollIntoView(this.scrollOptions), this.delay),
      // )
    },
  },
  mounted() {
    eventBus.on('scroll-anchor', evt => {
      console.log(evt)
      this.check(evt.charAt(0) === '#' ? evt.substring(1) : evt)
    })
    // this.checkMounted()
  },
  created() {},
}
</script>

<style lang="scss">
.scroll-anchor {
  display: block;
  position: relative;
  pointer-events: none;
}
</style>
