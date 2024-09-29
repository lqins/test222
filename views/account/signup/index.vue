<template>
  <div class="account flex-center">
    <Leftbox />
    <div class="right-box flex-center">
      <div class="tab-box">
        <Toptitle :big="$t('account.Register Swish')" :line="$t('common.signup')" />

        <!-- 注册 -->
        <el-form ref="ruleFormRef" :model="ruleForm" :rules="rules">
          <el-form-item label="" prop="account">
            <div class="form-title">{{ $t('account.Email') }}</div>
            <el-input v-model="ruleForm.account" :placeholder="$t('account.emailAd')" />
          </el-form-item>
          <el-form-item label="" prop="code">
            <div class="form-title">{{ $t('account.Code') }}</div>

            <div class="flex">
              <el-input class="input-code" v-model="ruleForm.code" :placeholder="$t('account.codeAd')" />
              <SendCode ref="sendCodeComponent" :account="ruleForm.account" />
            </div>
          </el-form-item>
          <el-form-item label="" prop="password">
            <div class="form-title">{{ $t('account.Password') }}</div>
            <el-input
              v-model="ruleForm.password"
              :type="passwordVisible ? 'text' : 'password'"
              :placeholder="$t('account.Password')"
            >
              <template #suffix>
                <img
                  :src="passwordVisible ? EyeClosedIcon : EyeIcon"
                  alt=""
                  @click="togglePassword(1)"
                  lass="password-icon"
                />
              </template>
            </el-input>
            <div
              class="pswRule"
              :class="{
                red: pswError
              }"
            >
              {{ $t('account.passwordsRule') }}
            </div>
          </el-form-item>
          <el-form-item label="" prop="passwordCheck">
            <div class="form-title">{{ $t('Confirm Password') }}</div>

            <el-input
              v-model="ruleForm.passwordCheck"
              :placeholder="$t('account.Password2')"
              :type="passwordVisibleCheck ? 'text' : 'password'"
            >
              <template #suffix>
                <img
                  :src="passwordVisibleCheck ? EyeClosedIcon : EyeIcon"
                  alt=""
                  @click="togglePassword"
                  lass="password-icon"
                />
              </template>
            </el-input>
          </el-form-item>

          <el-form-item>
            <div @click="submitForm" class="btn-blue">
              {{ $t('common.signup') }}
            </div>
            <div class="login-txt">
              {{ $t('account.Already') }}

              <span @click="goLogin" class="blue cursor">{{ $t('common.login') }}</span>
            </div>
            <div class="check-box flex-center" ref="shakeBox">
              <el-checkbox v-model="checked">
                <div class="login-txt checked-txt">
                  {{ $t('account.Agree to the') }}
                  <i @click="goLogin" class="blue cursor">{{ $t('account.User Privacy') }}</i>
                </div></el-checkbox
              >
            </div>
          </el-form-item>
        </el-form>
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
import I18n from '@/locale/index'
import Leftbox from '../components/Leftbox.vue'
import EyeIcon from '@/assets/img/icon/EyeIcon.png'
import EyeClosedIcon from '@/assets/img/icon/EyeClosedIcon.png'

import { validateEmail } from '@/utils/verify'
import SendCode from '@/components/SendCode/index.vue'
import Toptitle from '../components/Toptitle.vue'
import { register } from '@/api/user'

// import logoSrc from "@/assets/img/icon/logo.png";
const router = useRouter()

const btnLoading = ref(false)
const ruleFormRef = ref(null)
const ruleForm = reactive({
  account: '',
  password: '',
  passwordCheck: '',
  code: ''
})
const checked = ref(false)

const rules = {
  account: [{ required: true, validator: validateEmail, trigger: 'blur' }],
  password: [{ validator: validatePass, trigger: 'blur' }],
  passwordCheck: [{ validator: validateCheckPass, trigger: 'blur' }],
  code: [{ validator: validateCode, trigger: 'blur' }]
}

const pswError = ref(false)
function validatePass(rule: any, value: any, callback: any) {
  const regex = /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[!@$.%^&*+#])[A-Za-z\d!@$.%^&*+#]{8,}$/

  if (!value || !regex.test(value)) {
    pswError.value = true
    callback()
  } else {
    pswError.value = false
    callback()
  }
}

// 验证密码确认
function validateCheckPass(rule: any, value: any, callback: any) {
  if (!value || ruleForm.password !== ruleForm.passwordCheck) {
    callback(new Error(I18n.global.t('account.passwordsCheck')))
  } else {
    callback()
  }
}
function validateCode(rule: any, value: any, callback: any) {
  if (value === '') {
    callback(new Error(I18n.global.t('account.code')))
  } else {
    callback()
  }
}
const shakeBox = ref(null)
async function submitForm() {
  if (!checked.value) {
    // 移除 shake-animation 类名
    shakeBox.value.classList.remove('shake-animation')
    // 强制重绘（Reflow）
    void shakeBox.value.offsetWidth
    // 重新添加 shake-animation 类名，触发动画
    shakeBox.value.classList.add('shake-animation')
    return
  }
  await ruleFormRef.value.validate(async (valid, fields) => {
    if (valid) {
      if (btnLoading.value) return
      btnLoading.value = true
      try {
        let res: any = await register({
          email: ruleForm.account,
          code: Number(ruleForm.code),
          password: ruleForm.password,
          password2: ruleForm.passwordCheck
        })
        console.log('res', res)
        if (res.code === 0) {
          // 注册成功
          ElMessage({
            message: 'success!',
            grouping: true,
            type: 'success'
          })
          goLogin()
        }
      } catch (error) {
        console.error('Error:', error)
      } finally {
        btnLoading.value = false
      }
    } else {
      console.log('error submit!', fields)
    }
  })
}

const codeFlag = ref(false)

function goLogin() {
  router.push({
    path: '/login'
  })
}
const passwordVisible = ref(false)
const passwordVisibleCheck = ref(false)

function togglePassword(type?: any) {
  if (type == 1) {
    passwordVisible.value = !passwordVisible.value
  } else {
    passwordVisibleCheck.value = !passwordVisibleCheck.value
  }
}
</script>

<style lang="scss" scoped>
@import '../common.scss';
</style>
