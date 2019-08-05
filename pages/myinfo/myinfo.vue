<template>
  <form>
    <div class="field">
      <label class="label">昵称:</label>
    </div>
    <label class="label">{{userName}}</label>
    <view  class="line"></view>
    <view class="hr"></view>
    <view class="top">
      <view class="top-text">性别:</view>
      <!-- 下拉框 -->
      <view class="top-selected" @click="bindShowMsg">
        <text>{{grade_name}}</text>
      </view>
      <!-- 下拉需要显示的列表 -->
      <view class="select_box" wx:if="select">
        <view wx:for="grades" wx:key="unique">
          <view class="select_one" @click="mySelect" data-name="item">{{item}}</view>
        </view>
      </view>
    </view>
    <view  class="line"></view>
    <view class="hr"></view>
    <div class="field">
      <label class="label">个人介绍:</label>
      <view class="section">
        <textarea placeholder="请开始你的宣言" v-model="textarea" auto-focus/>
        <view class="btn-area"></view>
      </view>
    </div>
    <view  class="line"></view>
    <view class="hr"></view>
    <div class="control">
      <button class="button is-primary" @click="submit">Save</button>
    </div>
  </form>
</template>

<script>
import { mapState } from "vuex";

export default {
  computed: mapState(["forcedLogin", "hasLogin", "userName"]),
  data() {
    return {
      select: false,
      grade_name: "--请选择--",
      grades: ["男", "女", "未知"],
      textarea:'test'
    };
  },
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
                url: "../myinfo/myinfo"
              });
            } else {
              uni.navigateTo({
                url: "../myinfo/myinfo"
              });
            }
          }
        }
      });
    }
  },
  bindTextAreaBlur(e) {
    console.log(e.detail.value);
  },
  methods: {
    bindShowMsg() {
      this.select = true;
    },
    /**
     * 已选下拉框
     */
    mySelect(e) {
      console.log(e);
      var name = e.currentTarget.dataset.name;
      this.grade_name = name;
      this.select = false;
    },
    submit() {

    },

  }
};
</script>

<style>
.hr {
  width: 100%;
  height: 15px;
  background-color: #f4f5f6;
}
.line {
  border: 1px solid #ccc;
  opacity: 0.2;
}
</style>
