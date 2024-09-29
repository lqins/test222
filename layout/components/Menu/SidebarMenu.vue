<template>
  <template v-for="subItem in menuList" :key="subItem.id">
    <el-sub-menu v-if="subItem.children && subItem.children.length > 0" :index="subItem.path">
      <template #title>
        <!-- <el-icon>
          <component :is="subItem.meta.icon"></component>
        </el-icon> -->
        <SvgIcon v-if="subItem.icon" color="#fff" class="menu-icon" :name="subItem.icon" />
        <span class="menu-text">{{ subItem.meta.title }}</span>
      </template>
      <!-- 递归渲染子菜单 -->
      <SidebarMenu :menuList="subItem.children" :basePath="resolvePath(basePath, subItem.path)" />
    </el-sub-menu>
    <!-- 菜单项 -->
    <!-- <el-menu-item v-else :index="setPath(subItem.path)" @click="handleClickMenu(subItem)">
    
      <SvgIcon v-if="subItem.icon" color="#fff" class="menu-icon" :name="subItem.icon" />
      <template #title>
        <span class="menu-text">{{ subItem.meta.title }}</span>
      </template>
    </el-menu-item> -->
    <el-menu-item v-else :index="resolvePath(basePath, subItem.path)">
      <!-- 外部链接处理 -->
      <template v-if="subItem.meta.isLink">
        <a :href="subItem.meta.isLink" target="_blank" class="menu-link">
          <SvgIcon v-if="subItem.icon" color="#fff" class="menu-icon" :name="subItem.icon" />
          <span class="menu-text">{{ subItem.meta.title }}</span>
        </a>
      </template>
      <!-- 内部路由 -->
      <template v-else>
        <SvgIcon v-if="subItem.icon" color="#fff" class="menu-icon" :name="subItem.icon" />
        <span class="menu-text">{{ subItem.meta.title }}</span>
      </template>
    </el-menu-item>
  </template>
</template>

<script setup lang="ts">
import { useRouter } from 'vue-router'
import SvgIcon from '@/components/SvgIcon/index.vue'

let props = defineProps<{
  menuList: any[]
  basePath?: string
}>()

const router = useRouter()
const handleClickMenu = (subItem: any) => {
  if (subItem.meta.isLink) return window.open(subItem.meta.isLink, '_blank')
  if (props.basePath) {
    router.push(props.basePath + '/' + subItem.path)
  } else {
    router.push(subItem.path)
  }
}

//解析路径，防止多余的斜杠
function resolvePath(basePath: string | undefined, routePath: string) {
  if (basePath) {
    return `${basePath}/${routePath}`.replace(/\/+/g, '/')
  } else {
    return routePath
  }
}
</script>
<style lang="scss" scoped>
.el-menu--collapse {
  .menu-icon {
    position: absolute;
  }
}

.menu-text {
  margin-left: 16px;
}

.el-menu,
.el-menu--popup {
  .el-menu-item {
    background-color: rgb(19, 19, 19);

    &.is-active {
      background-color: rgb(0, 0, 0);

      &::before {
        position: absolute;
        top: 0;
        bottom: 0;
        left: 0;
        width: 4px;
        background: #409eff;
        content: '';
      }
    }
  }
}
</style>
