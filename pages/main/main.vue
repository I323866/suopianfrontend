<template>
  <view class="content">
    
    <view v-if="hasLogin" class="hello">
      <view class="title">您好 {{userName}}，您已成功登录。</view>
      <view class="ul">
        <view>这是 uni-app 带登录模板的示例App首页。</view>
        <view>在 “我的” 中点击 “退出” 可以 “注销当前账户”</view>
      </view>
    </view>
    <view v-if="!hasLogin" class="hello">
      <view class="title">您好 游客。</view>
      <view class="ul">
        <view>这是 uni-app 带登录模板的示例App首页。</view>
        <view>在 “我的” 中点击 “登录” 可以 “登录您的账户”</view>
      </view>
      <button type="primary" class="primary" @tap="testAPI">testAPI</button>
      <import src="../createpost/index.wxml"/>
      <view is="uploadimg"/>
    </view>

  </view>
	
</template>

<script>
import { mapState } from "vuex";

export default {
  computed: mapState(["forcedLogin", "hasLogin", "userName"]),
  onLoad() {
    if (!this.hasLogin) {
      uni.showModal({
        title: "未登录",
        content: "您未登录，需要登录后才能继续",
        /**
         * 如果需要强制登录，不显示取消按钮
         */
        showCancel: !this.forcedLogin,
        success: res => {
          if (res.confirm) {
            /**
             * 如果需要强制登录，使用reLaunch方式
             */
            if (this.forcedLogin) {
              uni.reLaunch({
                url: "../login/login"
              });
            } else {
              uni.navigateTo({
                url: "../login/login"
              });
            }
          }
        }
      });
    }
  },

  methods: {
    testAPI() {
      uni.request({
        url: this.$serverUrl + "/api/packages/",
        success: res => {
          console.log("老婆是豬");
          if (res.data.code !== 0) {
            return;
          }

          this.data = this.data.concat(res.data.data);
        },
        fail: () => {
          console.log("e");
        }
      });
    }
  }
};
</script>

<style>
.hello {
  display: flex;
  flex: 1;
  flex-direction: column;
}

.title {
  color: #8f8f94;
  margin-top: 50upx;
}

.ul {
  font-size: 30upx;
  color: #8f8f94;
  margin-top: 50upx;
}

.ul > view {
  line-height: 50upx;
}
.pic-title {
  margin: 20rpx;
  font-size: 30rpx;
  font-weight: 500;
  color: rgba(34, 34, 34, 1);
}

.pic-title .pic-title-content-right {
  color: #999;
}

.pic-title .pic-title-content-right-count {
  color: #fb0000;
}

.pic-content {
  margin-left: 10rpx;
  margin-right: 10rpx;
  display: flex;
  flex-wrap: wrap;
  margin-top: 60rpx;
  margin-bottom: 60rpx;
}

.pic-content .camera {
  width: 175rpx;
  height: 175rpx;
  background: rgba(255, 255, 255, 1);
  border-radius: 10rpx;
  position: relative;
  margin-top: 20rpx;
}

.pic-content .camera .img {
  width: 74rpx;
  height: 54rpx;
  position: absolute;
  top: 45%;
  left: 50%;
  transform: translate(-50%, -55%);
}

.pic-content .each-img {
  overflow: visible;
  margin-left: 10rpx;
  width: 175rpx;
  height: 175rpx;
  margin-top: 20rpx;
  position: relative;
}

.pic-content .each-img .delete-icon {
  background-color: white;
  border-radius: 50%;
  position: absolute;
  right: 5rpx;
  top: 0rpx;
  width: 38rpx;
  height: 38rpx;
}
</style>
