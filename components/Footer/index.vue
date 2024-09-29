<template>
  <div class="flex-center footer">
    <div class="webClick flex-center">
      <div class="link">{{ $t("common.Help") }}</div>
      <div class="link">{{ $t("common.Privacy") }}</div>
      <div class="link">{{ $t("common.TC") }}</div>
    </div>
    <div class="copyright">copyright © 2023 swish.pro</div>
  </div>
</template>

<script lang="ts" setup></script>

<style lang="scss" scoped>
.footer {
  flex-direction: column;
  .webClick {
    column-gap: 30px; /* 列间距 */
    margin-bottom: 20px;
  }
}
</style>
