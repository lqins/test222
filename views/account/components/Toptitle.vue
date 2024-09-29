<template>
  <div class="flex-center title-box">
    <div class="tab-title font-medium">{{ big }}</div>
    <div class="little-title">{{ small }}</div>
    <div class="tip-title flex-center" v-if="lineFlag">
      <i></i>
      <span>{{ line }}</span>
      <i></i>
    </div>
  </div>
</template>

<script lang="ts" setup>
import I18n from '@/locale/index'

import logoSrc from '@/assets/img/icon/logo.png'
interface propsType {
  big?: string
  small?: string
  line?: string
  lineFlag?: boolean
}
const props = withDefaults(defineProps<propsType>(), {
  big: I18n.global.t('account.welcome'),
  small: I18n.global.t('account.title'),
  line: I18n.global.t('common.login'),
  lineFlag: true
})
</script>

<style lang="scss" scoped>
.title-box {
  flex-direction: column;
}
.tab-title {
  font-size: 36px;
  color: #1d2129;
  margin-bottom: 20px;
}
.little-title {
  font-weight: 400;
  font-size: 14px;
  color: #4e5969;
  line-height: 18px;
  margin-bottom: 30px;
}
.tip-title {
  margin-bottom: 24px;
  i {
    display: inline-block;
    width: 146px;
    height: 1px;
    background-color: #e7e7e9;
  }
  span {
    margin: 0 6px;
    font-size: 14px;
    color: #cccccc;
  }
}
</style>
