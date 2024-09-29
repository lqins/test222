<template>
  <div class="flex-center account">
    <Leftbox />
    <div class="right-box flex-center">
      <div class="tab-box">
        <Toptitle />
        <!-- 登录 -->
        <el-form ref="ruleFormRef" :model="ruleForm" :rules="rules">
          <el-form-item prop="account">
            <div class="form-title">{{ $t('account.Email') }}</div>
            <el-input v-model="ruleForm.account" :placeholder="$t('account.emailAd')" />
          </el-form-item>
          <el-form-item prop="password">
            <div class="form-title flex-between">
              <div class="form-title">{{ $t('account.Password') }}</div>
              <div class="blue-text cursor" @click="goForget">{{ $t('account.Forget') }}</div>
            </div>

            <el-input
              :type="passwordVisible ? 'text' : 'password'"
              v-model="ruleForm.password"
              :placeholder="$t('account.Password')"
            >
              <template #suffix>
                <img
                  :src="passwordVisible ? EyeClosedIcon : EyeIcon"
                  alt=""
                  @click="togglePassword"
                  lass="password-icon"
                />
              </template>
            </el-input>
          </el-form-item>

          <el-form-item>
            <div v-if="loginError" class="error-tip">{{ $t('account.loginError') }}</div>
            <div @click="submitForm" class="btn-blue">
              {{ $t('common.login') }}
            </div>
            <div class="login-txt">
              {{ $t('account.accountNone') }}

              <span @click="goSignup" class="blue cursor">{{ $t('common.signup') }}</span>
            </div>
          </el-form-item>
        </el-form>
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
import I18n from '@/locale/index'
import { validateEmail } from '@/utils/verify'
import Leftbox from '../components/Leftbox.vue'
import Toptitle from '../components/Toptitle.vue'
import { useUserStore } from '@/store/user'
import { getParams } from '@/utils/index'
import { login } from '@/api/user'
import EyeIcon from '@/assets/img/icon/EyeIcon.png'
import EyeClosedIcon from '@/assets/img/icon/EyeClosedIcon.png'

const router = useRouter()
const route = useRoute()
const redirect = route.query.redirect

const { setUserInfo } = useUserStore()

const btnLoading = ref(false)
const ruleFormRef = ref()
const ruleForm = reactive({
  account: '',
  password: ''
})
const rules = {
  account: [{ validator: validateEmail, trigger: 'blur', message: '' }],
  password: [{ validator: validatePass, trigger: 'blur', message: '' }]
}

const passwordVisible = ref(false)
const pswError = ref(false)
function validatePass(rule: any, value: any, callback: any) {
  const regex = /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&.])[A-Za-z\d@$!%*?&.]{8,}$/

  if (!value || !regex.test(value)) {
    pswError.value = true
    callback(new Error(rule.message))
  } else {
    pswError.value = false
    callback()
  }
}
function togglePassword() {
  passwordVisible.value = !passwordVisible.value
}
const loginError = ref(false)

async function submitForm() {
  await ruleFormRef.value.validate(async (valid, fields) => {
    if (valid) {
      if (btnLoading.value) return
      btnLoading.value = true
      try {
        let res: any = await login({
          email: ruleForm.account,
          password: ruleForm.password
        })

        if (res.code === 0) {
          const token = res.data.token
          const profile = res.data.profile
          // 登录成功

          setUserInfo(profile, token)
          let redirectStr = decodeURIComponent(redirect as string)
          if (redirectStr.includes('login') || redirectStr === 'undefined') {
            redirectStr = ''
          }

          console.log('redirectStr', redirectStr)

          const queryStr = redirectStr.split('?')?.[1]
          let queryParams = {}

          if (queryStr) {
            console.log('getParams', getParams(queryStr))

            if (getParams(queryStr)) {
              queryParams = getParams(queryStr)
            }
          }

          router.push({
            path: redirectStr ? redirectStr : '/admin/dashboard',
            query: {
              ...queryParams
            }
          })
        } else {
          ElMessage.error(I18n.global.t('account.loginError'))
        }
      } catch (e) {
        console.error(e)
      } finally {
        btnLoading.value = false
      }
    } else {
      console.log('error submit!', fields)
      loginError.value = true
    }
  })
}

function goSignup() {
  router.push({
    path: '/signup'
  })
}
function goForget() {
  router.push({
    path: '/account/forget'
  })
}
</script>

<style lang="scss" scoped>
@import '../common.scss';
</style>
