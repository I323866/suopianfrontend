<template>
  <div class="mvpue-picker">
    <div>
      <text class="input">收件人：</text>
      <input class="input"/>
    </div>
    <view class="line"></view>

    <div>
      <text class="input">省市区：</text>
      <input class="input" @click="showMulLinkageThreePicker">{{pickerText}}</input>
    </div>
    <view class="line"></view>

    <div>
      <text class="input">具体地址：</text>
      <input class="input" type="text"></input>
    </div>
    <view class="line"></view>

    <div>
      <text class="input">邮编：</text>
      <uni-number-box type ="number" @input="bindChange" maxlength="6" />
    </div>
    <view class="line"></view>
    <div class="page-bd">
      <button type="default" @click="showMulLinkageThreePicker">三级联动选择</button>
    </div>
    <mpvue-picker
      ref="mpvuePicker"
      :mode="mode"
      :deepLength="deepLength"
      :pickerValueDefault="pickerValueDefault"
      :themeColor="themeColor"
      @onChange="onChange"
      @onConfirm="onConfirm"
      @onCancel="onCancel"
      :pickerValueArray="pickerValueArray"
    ></mpvue-picker>
  </div>
</template>

<script>
import uniNumberBox from "@/components/uni-number-box/uni-number-box.vue"
import mpvuePicker from "@/components/mpvue-picker/mpvuePicker.vue";
import addresslist from "./addresslist.json";
// import mpvuePicker from 'mpvue-picker';
export default {
  components: {
    mpvuePicker,
    uniNumberBox
  },
  data() {
    return {
      mode: "selector",
      addresslist,
      numberValue:1,
      deepLength: 0, // 几级联动
      pickerValueDefault: [], // 初始化值
      pickerValueArray: [], // picker 数组值
      pickerText: "",
      themeColor: "" // 颜色主题
    };
  },

  methods: {
    				bindChange(value){
				console.log(value)
			},
    onChange(e) {
      console.log(e);
    },
    onCancel(e) {
      console.log(e);
    },
    handleInput(e) {
      let a = e.key.replace(/[^\d]/g, "");
      if (!a) {
        e.preventDefault();
      }
    },

    // 三级联动选择
    showMulLinkageThreePicker() {
      this.pickerValueArray = this.addresslist;
      this.mode = "multiLinkageSelector";
      this.deepLength = 3;
      this.pickerValueDefault = [1, 1, 1];
      this.themeColor = "#8D0177";
      this.$refs.mpvuePicker.show();
    },
    showPickerView() {
      this.$refs.mpvuePicker.show();
    },
    onConfirm(e) {
      console.log(e);

      this.pickerText = e.label;
    }
  }
};
</script>

<style>
.page-hd {
  padding: 40px;
}
.page-title {
  font-size: 20px;
  font-weight: 400px;
}
.page-bd {
  padding: 15px;
}
.picker-text,
.page__desc {
  margin-top: 10px;
}
button {
  margin-top: 15px;
}
input {
  margin-left: 15px;
  margin-right: 15px;
}
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