<template>
  <div class="login-container">
    <van-nav-bar class="page-nav-bar" title="登录"></van-nav-bar>

    <!--登陆表单-->
    <van-form ref="loginForm" @submit="onSubmit">
      <van-field
        v-model="user.mobile"
        :rules="userFormRules.mobile"
        maxlength="11"
        name="mobile"
        placeholder="请输入手机号"
      >
        <i slot="left-icon" class="toutiao icon-shouji"></i>
      </van-field>
      <van-field
        v-model="user.code"
        :rules="userFormRules.code"
        maxlength="6"
        name="code"
        placeholder="请输入验证码"
        type="number"
      >
        <i slot="left-icon" class="toutiao icon-yanzhengma"></i>
        <template #button>
          <!--time是倒计时时间-->
          <van-count-down
            v-if="isCountDownShow"
            :time="1000 * 60"
            format="sss"
            @finish="isCountDownShow=false"
          />
          <van-button
            v-else
            class="send-sms-btn"
            native-type="button"
            round
            size="small"
            type="default"
            @click="onSendSms"
          >发送验证码
          </van-button>
        </template>
      </van-field>
      <div style="margin: 16px;">
        <van-button block native-type="submit" type="info">登录</van-button>
      </div>
    </van-form>
  </div>
</template>

<script>
import { login, sendSms } from '@/api/user'

export default {
  name: 'index',
  data () {
    return {
      user: {
        mobile: '13911111111', // 手机号
        code: '246810' // 验证码
      },
      userFormRules: {
        mobile: [{
          required: true,
          message: '手机号不能为空'
        }, {
          pattern: /^1[3|5|7|8]\d{9}$/,
          message: '手机号格式错误'
        }],
        code: [{
          required: true,
          message: '验证码不能为空'
        }, {
          pattern: /^\d{6}$/,
          message: '验证码格式错误'
        }]
      },
      isCountDownShow: false // 是否展示倒计时
    }
  },
  methods: {
    async onSubmit () {
      // const user = this.user
      this.$toast.loading({
        message: '登录中...',
        forbidClick: true,
        duration: 0
      })
      // TODO: 2. 表单验证
      try {
        const { data } = await login(this.user)
        this.$store.commit('setUser', data.data)
        this.$toast.success('登录成功')
        this.$router.back()
      } catch (err) {
        if (err.response.status === 400) {
          this.$toast.fail('手机号或验证码错误')
        } else {
          this.$toast.fail('登录失败，请稍后重试')
        }
      }
    },

    async onSendSms () {
      // 1. 校验手机号
      try {
        await this.$refs.loginForm.validate('mobile')
        console.log('yes')
      } catch (err) {
        return console.log('验证失败', err)
      }
      // 2. 通过显示倒计时
      this.isCountDownShow = true
      // 3. 请求发送验证码
      try {
        const res = await sendSms(this.user.mobile)
        console.log('发送成功', res)
      } catch (err) {
        console.log('发送失败', err)
        if (err.response.status === 429) {
          this.isCountDownShow = false
          this.$toast('凑尼玛，慢点')
        } else {
          this.$toast('嘤嘤嘤，发送失败了')
        }
      }
    }
  }
}
</script>

<style lang="less" scoped>
.login-container {
  .toutiao {
    font-size: 37px;
  }

  .send-sms-btn {
    width: 152px;
    height: 46px;
    line-height: 46px;
    background-color: #ededed;
    font-size: 22px;
    color: #666666;
  }
}
</style>
