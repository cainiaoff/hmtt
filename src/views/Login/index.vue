<template>
  <div>
    <van-nav-bar title="登陆" @click-left="$router.back()">
      <van-icon name="cross" slot="left" />
    </van-nav-bar>
    <template>
      <van-form @submit="onSubmit" ref="form">
        <van-field
          v-model.trim="mobile"
          name="mobile"
          placeholder="请输入手机号"
          :rules="[
            { required: true, message: '请输入手机号' },
            {
              pattern: /^(?:(?:\+\00)86)?1\d{10}$/,
              message: '不符合手机的格式',
            },
          ]"
        >
          <i class="toutiao toutiao-shouji" slot="left-icon"></i>
        </van-field>
        <van-field
          v-model.trim="code"
          type="password"
          name="code"
          placeholder="请输入验证码"
          :rules="[
            { required: true, message: '验证码' },
            { pattern: /^\d{6}$/, message: '验证码长度必须是6位' },
          ]"
          ><i class="toutiao toutiao-yanzhengma" slot="left-icon"></i>
          <template #button>
            <van-count-down
              :time="time"
              format="ss s"
              v-if="isCountDownShow"
              @finish="isCountDownShow = false"
            />
            <van-button
              v-else
              round
              size="small"
              class="yzm"
              native-type="button"
              @click="onSendSms"
              >发送验证码</van-button
            >
          </template>
        </van-field>
        <div style="margin: 16px">
          <van-button
            round
            block
            type="info"
            native-type="submit"
            class="login-btn"
            >提交</van-button
          >
        </div>
      </van-form>
    </template>
  </div>
</template>

<script>
import { getSmsCode, login } from '@/api/user'
export default {
  created () { },
  data () {
    return {
      mobile: '',
      code: '246810',
      time: 6 * 1000,
      isCountDownShow: false
    }
  },
  methods: {
    async onSubmit (values) {
      try {
        const res = await login(values)
        this.$store.commit('setUser', res.data.data)
        this.$router.push({ name: 'my' })
      } catch (err) { console.log(err) }
    },
    async onSendSms () {
      try {
        await this.$refs.form.validate('mobile')
        this.isCountDownShow = true
        try {
          await getSmsCode(this.mobile)
          this.$toast.success('发送成功')
        } catch (err) {
          console.log(err)
          this.$toast.fail('发送失败，请重试')
        }
      } catch (err) {
        console.log(err)
        console.log('校验失败')
        // 轻提示
        this.$toast.fail('手机号格式不对')
      }
    }
  },
  computed: {},
  watch: {},
  filters: {},
  components: {}
}
</script>

<style scoped lang='less'>
.toutiao {
  font-size: 37px;
}
.yzm {
  width: 152px;
  height: 46px;
  background-color: #ededed;
  border-radius: 23px;
  font-size: 22px;
  color: #666;
  .van-button__text {
    zoom: 0.95;
  }
}
.login-btn {
  width: 694px;
  height: 88px;
  background-color: #6db4fb;
  border-radius: 10px;
}
.van-count-down {
  position: fixed;
  right: 18px;
}
.yzm {
  position: fixed;
  right: 10px;
}
</style>
