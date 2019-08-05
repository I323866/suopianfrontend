<template>
  <view>
    <view
      class="tab"
      :class="[{'active':index==tabIndex}]"
      @tap="toggleTab(item,index)"
      v-for="(item,index) in tabList"
      :key="index"
    >{{item.name}}</view>
    <view class="uni-padding-wrap uni-common-mt">
      <form @submit="formSubmit" @reset="formReset">
        <w-picker mode="region" :defaultVal="[8,0,1]" @confirm="onConfirm" ref="region"></w-picker>
        <label @click="toggleTab()" class="tab">{{resultInfo.result}}</label>
        <view class="uni-form-item uni-column">
          <view class="tab">地址：</view>
          <input name="address"  class="tab" placeholder="请输入详细地址" />
          <view class="tab">收件人：</view>
          <input name="receiver" class="tab" placeholder="请输入收件人" />
          <view class="tab">邮编：</view>
          <input
            class="tab"
            type="number"
            maxlength="6"
            name="zipcode"
            placeholder="请输入邮编"
          />
        </view>
        <view class="uni-btn-v">
          <button class="margin" formType="submit">确认</button>
          <button class="margin" type="default" formType="reset">Reset</button>
        </view>
      </form>
    </view>
  </view>
</template>

<script>
import wPicker from "@/components/w-picker/w-picker.vue";
export default {
  components: {
    wPicker
  },
  data() {
    return {
      title: "Hello",
      mode: "range",
      defaultVal: [8, 0, 1],
      tabList: [
        {
          mode: "region",
          name: "省市区："
        }
      ],
      tabIndex: 0,
      resultInfo: {
        result: "上海市市辖区徐汇区"
      }
    };
  },
  computed: {},
  methods: {
    toggleTab(item, index) {
      if (item) {
        this.tabIndex = index;
        this.mode = item.mode;
        this.defaultVal = item.value;
      } else {
        this.tabIndex = 0;
        this.mode = "region";
        this.defaultVal = "上海市市辖区徐汇区";
      }
      this.$refs[this.mode].show();
    },
    onConfirm(val) {
      console.log(val);
      this.resultInfo = val;
    },
    formSubmit(val) {
      console.log(val.detail.value);
    }
  }
};
</script>

<style>
.tab {
  padding: 20upx 0;
  font-size: 32upx;
  margin-top: 1px;
  margin-bottom: 1px;
  margin-left: 10px;
  margin-right: 10px;
}

.result {
  margin-top: 200upx;
  font-size: 32upx;
  margin: 3px;
}
.margin {
  margin: 3px;
}
</style>
