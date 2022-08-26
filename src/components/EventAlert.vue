<template>
  <div
    :style="{ backgroundColor: bgColor, borderColor: borderColor, color: textColor }"
    class="p-2 font-bold rounded-[8px] border-2 flex mb-2 text-sm items-center h-auto mx-auto w-[240px] mt-1 cursor-pointer text-center"
    @click="redirect"
  >
    {{ messageText }}
  </div>
</template>

<script>
export default {
  props: {
    bgColor: {
      type: String,
      default: 'white'
    },
    textColor: {
      type: String,
      default: 'black'
    },
    borderColor: {
      type: String,
      default: '#FF912380'
    },
    messageText: {
      type: String,
      default: ''
    },
    link: {
      type: String,
      default: ''
    }
  },
  methods: {
    redirect () {
      if (this.link.length) {
        window.open(this.link)
      }
    }
  }
}
</script>
