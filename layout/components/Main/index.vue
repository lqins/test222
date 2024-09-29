<template>
  <router-view v-if="!routeInfo.meta.isIframe" v-slot="{ Component, route }">
    <!-- <transition appear name="fade-transform" mode="out-in"> -->
    <!-- <keep-alive :include="keepAliveStore.keepAliveName">
          <component :is="Component" v-if="isRouterShow" :key="route.path" />
        </keep-alive> -->
    <keep-alive :max="keepAliveMax">
      <component :is="Component" v-if="route.meta.keepAlive" :key="route.path" />
    </keep-alive>
    <component :is="Component" v-if="!route.meta.keepAlive" :key="route.path" />
    <!-- </transition> -->
  </router-view>
  <FullIframe v-else :url="url" />
</template>
<script lang="ts" setup>
import FullIframe from '@/components/FullIframe/index.vue'
import { useRoute } from 'vue-router'
const routeInfo = useRoute()
const url = routeInfo.meta.url as string
const keepAliveMax = 1
</script>
