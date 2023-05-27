<template>
  <div class="l-main">
    <slot></slot>
  </div>
</template>

<script setup lang="ts" name="Lmain"></script>

<style scoped lang="scss">
@include b(l-main) {
  flex: 1;
  overflow: auto;
}
</style>
