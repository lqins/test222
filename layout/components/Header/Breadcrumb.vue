<template>
  <el-breadcrumb :separator-icon="ArrowRight" class="bread">
    <!-- <el-breadcrumb-item v-for="item in routers" :key="item.path" :to="{ path: item?.path }"> -->
    <el-breadcrumb-item v-for="item in routers" :key="item.path">
      {{ item?.meta?.title }}
    </el-breadcrumb-item>
  </el-breadcrumb>
</template>

<script lang="ts" setup>
import { useRouter } from 'vue-router'
import { ArrowRight } from '@element-plus/icons-vue'
import { computed } from 'vue'
const router = useRouter()
// 当前路由的匹配记录
// console.log(router.currentRoute.value.matched)
const routers = computed(() => {
  // 过滤掉没有meta的
  return router.currentRoute.value.matched.filter(item => item.meta.title)
})
</script>
