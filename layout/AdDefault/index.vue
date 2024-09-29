<template>
  <el-container class="layout">
    <el-aside>
      <div class="menu" :style="{ width: menuCollapse ? '65px' : '210px' }">
        <el-scrollbar>
          <div class="logoBig" @click="goHome" :style="{ width: menuCollapse ? '65px' : '210px' }">
            <img src="@/assets/img/icon/logo.png" alt="" />
          </div>

          <el-menu
            :default-active="activeMenu"
            :router="true"
            :collapse-transition="false"
            :unique-opened="true"
            background-color="#191a20"
            text-color="#bdbdc0"
            active-text-color="#ffffff"
            :collapse="menuCollapse"
          >
            <SidebarMenu :menuList="authMenuList" />
          </el-menu>
        </el-scrollbar>
      </div>
    </el-aside>
    <el-container>
      <el-header><Header /></el-header>
      <el-main class="main-wrap"><Main /></el-main>
    </el-container>
  </el-container>
</template>
<script lang="ts" setup>
import { computed } from 'vue'
import SidebarMenu from '../components/Menu/SidebarMenu.vue'
import Header from '../components/Header/index.vue'
import Main from '../components/Main/index.vue'
import { storeToRefs } from 'pinia'
import { useRoute } from 'vue-router'
import { useRouter } from 'vue-router'

import { useCommonStore } from '@/store/common'
import { useUserStore } from '@/store/user'
const { menuCollapse } = storeToRefs(useCommonStore())
const { authMenuList } = storeToRefs(useUserStore())

const route = useRoute()
const router = useRouter()

const activeMenu = computed(() => (route.meta.activeMenu ? route.meta.activeMenu : route.path))

function goHome() {
  router.push('/admin/dashboard')
}
</script>
<style lang="scss" scoped>
.el-container {
  width: 100%;
  height: 100vh;

  .main-wrap {
    word-break: break-word;
    position: relative;
    box-sizing: border-box;

    .el-header {
      display: flex;
      height: 55px;
      padding: 0;
      background-color: #fff;
      border-bottom: 1px solid #f1f1f1;
      box-sizing: border-box;
      align-items: center;
      justify-content: space-between;

      // :deep(.tool-bar-ri) {
      //   .toolBar-icon,
      //   .username {
      //     color: var(--el-text-color-primary);
      //   }
      // }
    }
  }

  .el-aside {
    width: auto;
    overflow: inherit;
    background-color: #191a20;

    .menu {
      display: flex;
      height: 100%;
      transition: all 0.3s ease;
      flex-direction: column;

      .el-scrollbar {
        height: calc(100% - 55px);
        height: 100%;

        .el-menu {
          overflow-x: hidden;
          border-right: none;
        }
      }

      .logo {
        height: 55px;
        border-bottom: 1px solid #282a35;
        box-sizing: border-box;

        span {
          font-size: 21.5px;
          font-weight: bold;
          color: #dadada;
          white-space: nowrap;
        }

        img {
          width: 28px;
          object-fit: contain;
          margin-right: 6px;
        }
      }
    }
  }
}
.logoBig {
  img {
    width: 100%;
    height: 60px;
    object-fit: contain;
    cursor: pointer;
  }
}
</style>
