<template>
  <div class="home-page-wrap">
    <Header />

    <div class="box"></div>
  </div>
</template>
<script lang="ts" setup>
import Header from '@/components/Header/index.vue'
</script>
<style lang="scss" scoped>
.home-page-wrap {
  position: relative;
  height: 200vh;
  font-size: 28px;

  .box {
    width: 100%;
    height: 100px;
    background-color: aqua;
  }
}
</style>
