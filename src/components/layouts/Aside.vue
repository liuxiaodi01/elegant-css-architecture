<template>
  <div class="l-aside">
    <slot></slot>
  </div>
</template>

<script setup lang="ts" name="Laside"></script>

<style scoped lang="scss">
@include b(l-aside) {
  flex-shrink: 0;
  overflow: auto;
}
</style>
