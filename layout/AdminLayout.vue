<template>
  <component :is="LayoutComponents[layout]" />
</template>
<script lang="ts" setup>
import { type Component } from 'vue'
// import { listTreeByUser } from '@/api/user'
// import { formatMenuList } from '@/utils'
import Default from './AdDefault/index.vue'

const layout = 'default'

const LayoutComponents: { [key: string]: Component } = {
  default: Default
}

// })
</script>
