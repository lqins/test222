<template>
  <iframe :src="url" frameborder="0" class="full-iframe"></iframe>
</template>

<script setup lang="ts" name="FullIframe">
defineProps({
  url: {
    type: String,
    default: ''
  }
})
</script>

<style scoped lang="scss">
.full-iframe {
  width: 100%;
  height: 100%;
}
</style>
