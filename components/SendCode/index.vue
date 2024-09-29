<template>
  <el-button
    class="send-code-btn btn-blue"
    :class="{ disabled: current.seconds }"
    :disabled="current.seconds > 0 ? true : false"
  >
    <span v-if="current.seconds === 0" @click="sendVerifyCode">
      {{ $t('verify.code') }}
    </span>
    <span v-else class="count-down">{{ $t('verify.resend') }}{{ current.seconds }}s</span>
  </el-button>
</template>

<script lang="ts" setup>
import { useCountDown } from '@/hooks/useCountDown'
import { getRegisterCode } from '@/api/system'

const props = defineProps({
  account: {
    type: String
  }
})

const { start, pause, reset, current } = useCountDown({
  time: 60 * 1000
  // millisecond: props.millisecond,
  // onChange: current => emit('change', current),
  // onFinish: () => emit('finish')
})
// start();
async function sendVerifyCode() {
  console.log('sendVerifyCode')
  if (props.account === '' || props.account == undefined) {
    console.log('sendVerifyCode-if')
    return
  }
  reset()
  start()
  const params = {
    email: props.account
  }

  let res = await getRegisterCode(params)
  console.log(params)
}
</script>

<style lang="scss" scoped>
.send-code-btn {
  height: 48px;
  margin-left: 8px;
  span {
    font-size: 16px;
    color: #ffffff;
  }
}
.count-down {
  color: #000 !important;
}
</style>
