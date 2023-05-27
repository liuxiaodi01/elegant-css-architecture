<template>
  <div
    class="l-header"
    :class="{
      'l-header--fixed': fixed,
    }"
  >
    <slot></slot>
  </div>
</template>

<script setup lang="ts" name="Lheader">
defineProps({
  fixed: {
    type: Boolean,
    default: false,
  },
});
</script>

<style scoped lang="scss">
@include b(l-header) {
  @include m(fixed) {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    z-index: 11;
  }
}
</style>
