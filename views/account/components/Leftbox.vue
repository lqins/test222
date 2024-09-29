<template>
  <div class="imgbox">
    <div class="flex-center footer">
      <div class="webClick flex-center">
        <div class="link">{{ $t('common.Help') }}</div>
        <div class="link">{{ $t('common.Privacy') }}</div>
        <div class="link">{{ $t('common.TC') }}</div>
      </div>
      <div class="copyright">copyright © 2023 swish.pro</div>
    </div>
  </div>
</template>

<script lang="ts" setup></script>

<style lang="scss" scoped>
.imgbox {
  position: relative;
  width: 60%;
  min-height: 100vh;
  max-height: 100vh;
  background-image: url('https://images.unsplash.com/photo-1725203653093-3af9174d6bbe?q=80&w=2864&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  position: relative;
  overflow: hidden;
  z-index: 1;
  @media (max-width: 768px) {
    display: none !important;
    z-index: 1;
  }
  img {
    width: 100%;
    object-fit: cover;
    min-height: 100vh;
    max-height: 100vh;
  }
}
.footer {
  font-weight: 400;
  font-size: 14px;
  color: #fff;
  opacity: 0.7;
  position: absolute;
  bottom: 36.4px;
  width: 100%;
  flex-direction: column;
  .webClick {
    column-gap: 30px; /* 列间距 */
    margin-bottom: 16px;
  }
}
</style>
