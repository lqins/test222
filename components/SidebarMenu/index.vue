<template>
  <el-menu
    :default-active="activeMenu"
    router
    background-color="#304156"
    text-color="#bfcbd9"
    active-text-color="#409eff"
  >
    <el-menu-item index="/admin/dashboard">
      <i class="el-icon-menu"></i>
      <span>Dashboard</span>
    </el-menu-item>
    <el-menu-item index="/admin/settings">
      <i class="el-icon-setting"></i>
      <span>Settings</span>
    </el-menu-item>
    <!-- 其他菜单项 -->
  </el-menu>
</template>

<script lang="ts" setup>
import { useRoute } from "vue-router";
const route = useRoute();
const activeMenu = route.path;
</script>

<style scoped>
/* 自定义样式 */
</style>
