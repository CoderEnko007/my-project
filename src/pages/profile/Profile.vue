<template>
  <div class="container">
    <div class="userinfo">
      <img :src="userinfo.avatarUrl">
      <p>{{userinfo.nickName}}</p>
      <button v-if='!userinfo.openId&&canIUse'
              open-type='getUserInfo'
              lang="zh_CN"
              @getuserinfo='login'>点击登录</button>
      <p v-if='!canIUse'>请升级微信版本</p>
    </div>
  </div>
</template>

<script>
  import qcloud from 'wafer2-client-sdk'
  import  config from '@/config'

  export default {
    data() {
      return {
        userinfo: {
          avatarUrl: '../../../static/img/me.png',
          nickName: '',
        },
        canIUse: wx.canIUse('button.open-type.getUserInfo')
      }
    },
    methods: {
      login() {
        let user = wx.getStorageSync('userinfo')
        const self = this
        if(!user) {
          qcloud.setLoginUrl(config.loginUrl)
          qcloud.login({
            success: userInfo => {
              qcloud.request({
                url: config.userUrl,
                login: true,
                success: userRes => {
                  console.log('登陆成功', userRes)
                  wx.setStorageSync('userinfo',userRes.data.data)
                  self.userinfo = userRes.data.data
                },
                fail: () => {
                  console.log('登陆失败')
                }
              })
            }
          })
        }
      }
    },
    onShow() {
      console.log('onShow()')
      let userinfo = wx.getStorageSync('userinfo')
      if (userinfo) {
        this.userinfo = userinfo
      }
    }
  }
</script>

<style scoped>

img {
  width: 150rpx;
  height: 150rpx;
  margin-left: 15rpx;
}
button {
  width: 30vw;
  background-color: #fff;
  color: #000;
  font-size: 14px;
}

</style>
