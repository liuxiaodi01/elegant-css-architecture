<template>
  <!-- <Hello></Hello> -->
  <div class="app-wrapper">qweqweqwe</div>
</template>
<script setup lang="ts">
import { onMounted } from "vue";
onMounted(() => {
  document.documentElement.setAttribute("data-theme", "defalut");
});
</script>
<style scoped lang="scss">
@include b(app-wrapper) {
  @include text-color(text-color);
}
</style>
