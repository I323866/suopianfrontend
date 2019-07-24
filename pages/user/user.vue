<template>
  <view class="content">
    <view class="btn-row">
      <button v-if="!hasLogin" type="primary" class="primary" @tap="bindLogin">登录</button>
    </view>
    <view v-if="hasLogin" class="hr"></view>
    <view v-if="hasLogin" class="item" v-on:click="navmyinfo" >
      <view class="title" v-on:click="navmyinfo">个人信息</view>
      <view class="detail2" >
        <text>></text>
      </view>
    </view>
    <view v-if="hasLogin" class="line"></view>
    <view v-if="hasLogin" class="hr"></view>
    <view v-if="hasLogin" class="item">
      <view class="title">我的帖子</view>
      <view class="detail2">
        <text>></text>
      </view>
    </view>
    <view v-if="hasLogin" class="line"></view>
    <view v-if="hasLogin" class="hr"></view>
    <view v-if="hasLogin" class="item">
      <view class="title">我的互寄</view>
      <view class="detail2">
        <text>></text>
      </view>
    </view>
    <view v-if="hasLogin" class="line"></view>
    <view v-if="hasLogin" class="hr"></view>
    <view v-if="hasLogin" class="item">
      <view class="title">我的互寄</view>
      <view class="detail2">
        <text>></text>
      </view>
    </view>
    <view v-if="hasLogin" class="line"></view>
    <button v-if="hasLogin" type="default" @tap="bindLogout">退出登录</button>
  </view>
</template>

<script>
import { mapState, mapMutations } from "vuex";

export default {
  computed: {
    ...mapState(["hasLogin", "forcedLogin"])
  },
  methods: {
    ...mapMutations(["logout"]),
    bindLogin() {
      uni.navigateTo({
        url: "../login/login"
      });
    },
    navmyinfo() {
      uni.navigateTo({
        url: "../myinfo/myinfo"
      });
    },

    bindLogout() {
      this.logout();
      /**
       * 如果需要强制登录跳转回登录页面
       */
      if (this.forcedLogin) {
        uni.reLaunch({
          url: "../login/login"
        });
      }
    },
    kindToggle() {
      console.log("aaa");
    }
  }
};
</script>

<style>
.bg {
  height: 150px;
  background-color: #d53e37;
}

.head {
  display: flex;
  flex-direction: column;
}

.headIcon {
  display: flex;
  justify-content: center;
}

.login {
  display: flex;
  color: #fff;
  font-size: 15px;
  margin-top: 15rpx;
  justify-content: center;
}

.button {
  margin: 10px;
  font-size: 14px;
}

.head image {
  border-radius: 50%;
}

.hr {
  width: 100%;
  height: 15px;
  background-color: #f4f5f6;
}

.item {
  display: flex;
  flex-direction: row;
}

.title {
  padding-top: 15px;
  padding-bottom: 15px;
  padding-left: 15px;
  font-size: 15px;
}

.detail2 {
  font-size: 15px;
  position: absolute;
  right: 10px;
  height: 50px;
  line-height: 50px;
  color: #888;
}

.line {
  border: 1px solid #ccc;
  opacity: 0.2;
}
</style>
