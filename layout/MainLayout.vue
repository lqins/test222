<template>
  <div class="main-layout-wrap">
    <Header :key="route.fullPath" :headerType="headerType" v-if="!route.meta.hideHeader" />
    <router-view v-slot="{ Component }">
      <keep-alive>
        <component :is="Component" v-if="route.meta.keepAlive" :key="route.fullPath" />
      </keep-alive>
      <component :is="Component" v-if="!route.meta.keepAlive" :key="route.fullPath" />
    </router-view>

    <Footer v-if="!route.meta.hideFooter === true" />
  </div>
</template>
<script lang="ts" setup>
import Header from '@/components/Header/index.vue'
import Footer from '@/components/Footer/index.vue'
import { useRoute } from 'vue-router'
import { useCommonStore } from '@/store/common'

const { headerType } = storeToRefs(useCommonStore())

const route = useRoute()
</script>
<style lang="scss" scoped>
.main-layout-wrap {
  padding-top: 44px;
}
</style>
