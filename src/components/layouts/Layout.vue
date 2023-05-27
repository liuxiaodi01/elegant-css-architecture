<template>
  <div
    class="l-layout"
    :class="{
      'l-layout--horizontal': horizontal,
    }"
  >
    <slot></slot>
  </div>
</template>

<script setup lang="ts" name="Llayout">
defineProps({
  horizontal: {
    type: Boolean,
    default: false,
  },
});
</script>

<style scoped lang="scss">
@include b(l-layout) {
  display: flex;
  height: 100%;
  width: 100%;
  flex-direction: row;
  @include m(horizontal) {
    flex-direction: column;
  }
}
</style>
