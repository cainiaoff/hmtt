<template>
  <div>
    <!-- 登陆 -->
    <!-- 原写法 -->
    <!-- v-if="$store.state.user && $store.state.user.token" -->
    <div class="header header-login" v-if="user && user.token">
      <div class="avatar">
        <div class="left">
          <van-image
            round
            width="1.76rem"
            height="1.76rem"
            :src="userInfo.photo"
            @click="submit"
          />
          <span>{{ userInfo.name }}</span>
        </div>
        <span class="button">编辑资料</span>
      </div>
      <ul class="list">
        <li>
          <p>{{ userInfo.art_count }}</p>
          <p>头条</p>
        </li>
        <li>
          <p>{{ userInfo.fans_count }}</p>
          <p>粉丝</p>
        </li>
        <li>
          <p>{{ userInfo.follow_count }}</p>
          <p>关注</p>
        </li>
        <li>
          <p>{{ userInfo.like_count }}</p>
          <p>获赞</p>
        </li>
      </ul>
    </div>
    <!-- 未登录 -->
    <div class="header header-notlogin" v-else>
      <div class="inner" @click="$router.push('/login')">
        <img src="@/assets/mobile.png" alt="" />
        <p>登陆/注册</p>
      </div>
    </div>
    <van-grid :column-num="2">
      <van-grid-item text="收藏">
        <template #icon>
          <i class="toutiao toutiao-shoucang" style="color: #eb5253"></i>
        </template>
      </van-grid-item>
      <van-grid-item text="历史">
        <template #icon>
          <i class="toutiao toutiao-lishi" style="color: #ff9d1d"></i>
        </template>
      </van-grid-item>
    </van-grid>
    <van-cell-group>
      <van-cell title="消息通知" is-link />
      <van-cell title="小智同学" is-link />
    </van-cell-group>
    <van-button
      class="logout"
      type="default"
      block
      v-if="user && user.token"
      @click="logout"
      >退出登陆</van-button
    >
  </div>
</template>

<script>
import { getUserInfo } from '@/api/user'
import { mapState } from 'vuex'
export default {
  name: 'My',
  async created () {
    if (this.user && this.user.token) {
      try {
        const res = await getUserInfo()
        console.log('res', res)
        this.userInfo = res.data.data
      } catch (err) {
        console.log(err)
      }
    }
  },
  data () {
    return {
      userInfo: {}
    }
  },
  methods: {
    async logout () {
      try {
        await this.$dialog.confirm({ message: '确定退出吗' })
        this.$store.commit('setUser', {})
      } catch (err) { console.log(err) }
    }
  },
  computed: {
    ...mapState(['user'])
  },
  watch: {},
  filters: {},
  components: {}
}
</script>

<style scoped lang='less'>
.header {
  width: 750px;
  height: 401px;
  opacity: 0.8;
  background: rgba(50, 150, 250, 0.7) url("@/assets/banner.png") no-repeat;
  background-size: cover;
}
.header-notlogin {
  display: flex;
  justify-content: center;
  align-items: center;
  img {
    width: 132px;
    height: 132px;
    margin-bottom: 15px;
  }
  p {
    font-size: 28px;
    font-weight: 400;
    color: #fff;
    padding: 0;
    margin: 0;
  }
}
.header-login {
  padding-top: 116px;
  .avatar {
    margin-bottom: 55px;
    padding-left: 32px;
    padding-right: 33px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    .button {
      width: 115px;
      height: 32px;
      line-height: 32px;
      text-align: center;
      background: #fff;
      border-radius: 16px;
      font-size: 20px;
      font-weight: 400;
      color: #666;
    }
    .left {
      //图片对齐
      // .van-image {
      //   vertical-align: middle;
      // }
      display: flex;
      align-items: center;
      span {
        margin-left: 22px;
        font-size: 30px;
        font-weight: 400;
        color: #fff;
      }
    }
  }
}
.list {
  display: flex;
  li {
    flex: 1;
    text-align: center;
    P {
      color: #fff;
      font-weight: 400;
      &:nth-child(1) {
        font-size: 36px;
      }
      &:nth-child(2) {
        font-size: 23px;
      }
    }
  }
}
.toutiao {
  font-size: 45px;
}
// css图标颜色写法
// .van-grid-item {
//   &:nth-child(1) {
//     color: #eb5253;
//   }
//   &:nth-child(2) {
//     color: #ff9d1d;
//   }
// }
/deep/.van-grid-item__text {
  font-size: 28px;
}
button {
  display: block;
}
.van-cell-group {
  margin: 9px 0;
}
.logout {
  color: #d86262;
}
</style>
