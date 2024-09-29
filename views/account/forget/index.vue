<template>
  <div class="account flex-center">
    <Leftbox />

    <div class="right-box flex-center">
      <div class="tab-box reset-box">
        <Toptitle
          :big="$t('account.resetpswd')"
          :small="type === 0 ? $t('account.resetMsg') : $t('account.passwordsRule')"
          :lineFlag="false"
        />

        <el-form ref="formRef" :model="formModel" :rules="rules">
          <el-form-item v-if="type === 0" label="" prop="account">
            <div class="form-title">{{ $t('account.Email address') }}</div>
            <el-input v-model="formModel.account" :placeholder="$t('account.emailAd')" />
          </el-form-item>

          <el-form-item v-if="type !== 0" label="" prop="password">
            <div class="form-title">{{ $t('account.New password') }}</div>
            <el-input
              :type="passwordVisible ? 'text' : 'password'"
              v-model="formModel.password"
              :placeholder="$t('account.Enter password')"
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
          </el-form-item>

          <el-form-item v-if="type !== 0" label="" prop="passwordCheck">
            <div class="form-title">{{ $t('account.Confirm Password') }}</div>
            <el-input
              :type="passwordVisibleCheck ? 'text' : 'password'"
              v-model="formModel.passwordCheck"
              :placeholder="$t('account.Confirm Password')"
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
            <div class="reset-login">
              <el-button class="btn-blue" @click="submitForm" :disabled="codeFlag" type="primary">
                {{ type === 0 ? $t('account.SendEmail') : $t('account.resetpswd') }}
              </el-button>
            </div>
          </el-form-item>
        </el-form>

        <div class="login-txt">
          {{ $t('account.Return to') }}
          <span @click="goLogin" class="blue cursor">{{ $t('common.login') }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
import Leftbox from '../components/Leftbox.vue'
import I18n from '@/locale/index'
import Toptitle from '../components/Toptitle.vue'
import { validateEmail } from '@/utils/verify'
import { setForgetPasswordEmail, resetPassword } from '@/api/user'
import EyeIcon from '@/assets/img/icon/EyeIcon.png'
import EyeClosedIcon from '@/assets/img/icon/EyeClosedIcon.png'

const router = useRouter()
const route = useRoute()
const type = route.query.type || 0
const resetCode: any = route.query.resetCode || ''
const email: any = route.query.email || ''
//
const formRef = ref(null)
const formModel = reactive({
  account: '',
  password: '',
  passwordCheck: ''
})
const btnLoading = ref(false)

const codeFlag = ref(false)

// 校验规则
const rules = {
  account: [{ validator: validateEmail, trigger: 'blur' }],
  password: [{ validator: validatePass, trigger: 'blur' }],
  passwordCheck: [{ validator: validateCheckPass, trigger: 'blur' }]
}

// 验证密码格式
function validatePass(rule: any, value: any, callback: any) {
  const regex = /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&.])[A-Za-z\d@$!%*?&.]{8,}$/

  if (!value || !regex.test(value)) {
    callback(new Error(I18n.global.t('account.passwordsRule')))
  } else {
    callback()
  }
}

// 验证密码确认
function validateCheckPass(rule: any, value: any, callback: any) {
  if (!value || formModel.password !== formModel.passwordCheck) {
    callback(new Error(I18n.global.t('account.passwordsCheck')))
  } else {
    callback()
  }
}

// 提交表单
async function submitForm() {
  await formRef.value.validate(async (valid, fields) => {
    if (valid) {
      if (btnLoading.value) return
      btnLoading.value = true
      try {
        if (type === 0) {
          const res = await setForgetPasswordEmail({
            email: formModel.account
          })
          if (res.code === 0) {
            ElMessage.success(I18n.global.t('account.Password reset'))
          } else {
            ElMessage.error(I18n.global.t('account.sorry'))
          }
        } else {
          const res = await resetPassword({
            resetCode: resetCode,
            password: formModel.password,
            password2: formModel.passwordCheck
          })
          if (res.code === 0) {
            // 重置成功
            ElMessage.success(I18n.global.t('common.success'))

            goLogin()
          }
        }
      } catch (error) {
        console.error('Error:', error)
      } finally {
        btnLoading.value = false
      }
    } else {
      console.log('表单验证失败', fields)
    }
  })
}

function goLogin() {
  router.push({ path: '/login' })
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

.reset-login {
  width: 362px;
}

.tab-box {
  ::v-deep(.el-input) {
    max-width: 362px !important;
  }
  .btn-blue {
    margin-bottom: 0px !important;
  }
  ::v-deep(.title-box) {
    .little-title {
      margin-bottom: 18px;
    }
    .tab-title {
      margin-bottom: 24px;
    }
  }
}
</style>
