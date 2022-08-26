<template>
  <div
    class="flex items-center h-[40px] cursor-pointer hover:bg-white hover:rounded-[10px]"
    :class="{'bg-white rounded-[10px] text-[#424242]': selected, 'text-[#606061]': !selected}"
  >
    <div class="flex-none flex items-center justify-center h-[30px] w-[30px] ml-[5px] mr-[8px] relative">
      <div
        v-if="count > 0"
        class="min-w-[13px] min-h-[13px] absolute rounded-[3px] bg-[#4c4c4d] -top-0.5 -left-0.5 flex items-center justify-center"
      >
        <span
          class="text-white font-['Roboto'] text-[10px] leading-[12px] font-medium px-0.5"
        >
          {{ count }}
        </span>
      </div>
      <slot />
    </div>
    <div class="flex-initial flex items-center overflow-hidden w-full mr-[8px]">
      <p
        class="font-roboto text-[13px] leading-[15px] font-medium truncate"
      >
        {{ title }}
      </p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    count: {
      type: Number,
      default: 0
    },
    selected: {
      type: Boolean,
      default: false
    },
    title: {
      type: String,
      default: ''
    }
  }
}
</script>

<style scoped>

</style>
