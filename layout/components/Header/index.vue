<template>
  <div class="header-wrap">
    <div class="header-left">
      <SvgIcon
        :name="menuCollapse ? 'expand' : 'collapse'"
        class="switch-icon"
        @click="switchMenuCollapse(!menuCollapse)"
      />
      <Breadcrumb />
    </div>
    <div class="header-right">
      <el-dropdown trigger="click">
        <div class="user-info">{{ userInfo.username }}</div>

        <span class="el-dropdown-link"> 下拉菜单<i class="el-icon-arrow-down el-icon--right"></i> </span>
        <template #dropdown>
          <el-dropdown-menu>
            <el-dropdown-item @click="handleChangePassword">修改密码</el-dropdown-item>

            <el-dropdown-item @click="useLogout">退出</el-dropdown-item>
          </el-dropdown-menu>
        </template>
      </el-dropdown>
    </div>
  </div>
</template>
<script lang="ts" setup>
import { useLogout } from '@/hooks/auth'
import { storeToRefs } from 'pinia'
import { useUserStore } from '@/store/user'
import { useCommonStore } from '@/store/common'
import Breadcrumb from './Breadcrumb.vue'
import SvgIcon from '@/components/SvgIcon/index.vue'
// import ChangePassword from '@/components/ChangePassword/'
const { userInfo } = useUserStore()
const { menuCollapse } = storeToRefs(useCommonStore())
const { setMenuCollapse } = useCommonStore()
function switchMenuCollapse(bool: boolean) {
  setMenuCollapse(bool)
}
function handleChangePassword() {
  // ChangePassword().then(res => {
  //   console.log(res)
  // })
}
</script>
<style lang="scss" scoped>
.header-wrap {
  display: flex;
  width: 100%;
  height: 60px;
  padding: 0;
  background-color: #fff;
  border-bottom: 1px solid #d8d8d8;
  box-sizing: border-box;
  justify-content: space-between;
  align-items: center;

  .header-left {
    display: flex;
    align-items: center;
    padding-left: 10px;

    .switch-icon {
      width: 30px;
      height: 30px;
      margin-right: 40px;
      cursor: pointer;
    }
  }

  .header-right {
    display: flex;
    align-items: center;
    padding-right: 20px;
  }

  .user-info {
    margin-right: 10px;
    cursor: pointer;
  }
}
</style>
