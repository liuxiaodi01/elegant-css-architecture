<template>
  <div class="app-wrapper">
    <!-- 
    <Llayout horizontal>
      <Lheader fixed style="background-color: aqua">header</Lheader>
      <Lmain style="background-color: pink">
        <p>qwqweqweqw</p>
        <p>qwqweqweqw</p>
      </Lmain>
      <Lfooter>footer</Lfooter>
    </Llayout> 
      -->
    <l-layout>
      <L-aside bg-blue>
        <p>ddddddddd</p>
        <p>ddddddddd</p>
        <p>ddddddddd</p>
        <p>ddddddddd</p>
        <p>ddddddddd</p>
        <p>ddddddddd</p>
      </L-aside>
      <Lmain>
        <p bg-primary m-0 p-0>qwqweqweqw</p>
        <p bg-dark text-white m-0 p-0>qwqweqweqw</p>
      </Lmain>
    </l-layout>
  </div>
</template>

<script setup lang="ts">
import LAside from "./layouts/Aside.vue";
import Lfooter from "./layouts/Footer.vue";
import Lheader from "./layouts/Header.vue";
import Lmain from "./layouts/Main.vue";
import LLayout from "./layouts/Layout.vue";
const props = defineProps({
  id: {
    type: Number,
    default: 0,
  },
});
</script>

<style scoped lang="scss">
.app-wrapper {
  height: 100vh;
  width: 100vw;
}
</style>
