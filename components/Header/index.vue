<template>
  <div class="header-wrap">
    <img class="logo" src="@/assets/img/icon/logo.png" alt="" />
    <div class="right-box">
      <!-- <img class="logo-search" src="@/assets/img/icon/search.png" alt="" /> -->
      <div class="login-btn" @click="goLogin">{{ $t('common.login') }}</div>
      <div class="login-btn" @click="goSignup">{{ $t('common.signup') }}</div>

      <div class="login-btn" @click="switchLanguage">{{ language }}</div>
    </div>
  </div>
</template>
<script lang="ts" setup>
import { useCommonStore } from '@/store/common'
const router = useRouter()

const { language } = storeToRefs(useCommonStore())
const { setLanguage } = useCommonStore()
// function switchLanguage(val) {
//   let lang = language.value === "zh-hk" ? "en" : "zh-hk";
//   setLanguage(lang);
//   setTimeout(() => {
//     i18n.locale.value = val;
//   }, 0);
// }
function switchLanguage() {
  let lang = language.value === 'zh-hk' ? 'en' : 'zh-hk'
  setLanguage(lang)
  i18n.locale.value = lang
}
const i18n = useI18n()

function goLogin() {
  router.push({
    path: '/login'
  })
}
function goSignup() {
  router.push({
    path: '/signup'
  })
}
</script>
<style lang="scss" scoped>
.header-wrap {
  position: fixed;
  top: 0;
  left: 0;
  z-index: 99;
  display: flex;
  width: 100%;
  height: 44px;
  padding: 0 15px;
  font-size: 14px;
  background-color: rgb(255, 255, 255);
  box-shadow: 0 1px 10px #e0e0e0;
  justify-content: space-between;
  align-items: center;
  box-sizing: border-box;

  .logo {
    height: 24px;
  }

  .right-box {
    display: flex;
    align-items: center;

    .logo-menu {
      width: 24px;
      height: 24px;
      margin-left: 22px;
    }

    .login-btn {
      display: flex;
      height: 30px;
      padding: 0 13px;
      margin-left: 22px;
      font-size: 14px;
      align-items: center;
      border: 1px solid #333;
      border-radius: 30px;
    }

    .logo-search {
      width: 24px;
      height: 24px;
    }
  }
}
</style>
