<template>
	<view>
	<view class="title">主题内容</view>
		<!--相关链接  -->
	<m-input name="xgurl" placeholder='标题' class="xgurl" placeholder-class="xgurl-phcolor"></m-input>
	<form bindsubmit="formSubmit" bindreset="formReset">    
	<!--输入区域  -->
	<textarea name="userliuyan" placeholder="请输入内容" placeholder-class="phcolor" class="textarea" maxlength="noteMaxLen" bindinput="bindWordLimit"></textarea> 
	<!--字数统计  -->
	<text class="wordLimit ff-hv"></text>

	<view class="imgfile">图片上传</view>
	<!--上传并显示图片  -->
	<view style="width:90%;height:90px;margin:0 auto 30px;">
	<view @tap="uploadimg" class="imgselect"><image src="../../static/img/source.png"></image></view>
	</view>
	<!--获取上传成功的图片地址  -->
	<m-input type='text' value="source" name="filename" style="display:none;"></m-input>
	<!--提交按钮  -->
	<button @tap="formSubmit" formType='submit' class="btn">提交</button>    
	</form>
	</view>
</template>

<script>
import { mapState } from "vuex";
import mInput from '../../components/m-input.vue';
export default {
      components: {
            mInput
        },
  data() {
    return {
    post:'',
    　　//初始化为空
    noteMaxLen: 500, //备注最多500字数
    source: ''
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
    },

  /**
   * 上传图片
   */
  uploadimg() {
    var that = this;
    wx.chooseImage({  //从本地相册选择图片或使用相机拍照
      count: 9, // 默认9
      sizeType: ['original', 'compressed'], // 可以指定是原图还是压缩图，默认二者都有
      sourceType: ['album', 'camera'], // 可以指定来源是相册还是相机，默认二者都有

      success: function (res) {
        //console.log(res)
        //前台显示
        // that.setData({
          that.source = res.tempFilePaths
        // })

        // 返回选定照片的本地文件路径列表，tempFilePath可以作为img标签的src属性显示图片
        // var tempFilePaths = res.tempFilePaths
        wx.uploadFile({
          url: 'http://localhost:3000/api/photos/',
          filePath: that.source[0],
          name: 'file',
		  formData:{
			  user:"test",
			  data: that.source[0]
		  },
          success: function (res) {
            //打印
            console.log(res.data)
          }
        })
      }
    })
  },
    //表单提交
  formSubmit(e) {
    var that = this;
    var formData = e.detail.value; //获取表单所有input的值  
    wx.request({
      url: 'http://localhost:3000/api/feedbacks/',
      data: formData,
	  method: "POST",
      header: { 'Content-Type': 'application/json' },
      success: function (res) {
        console.log(res.data)
        wx.showToast({
          title: '已提交',
          icon: 'success',
          duration: 2000
        })
      }
    })
  }, 
    //字数限制  
  bindWordLimit() {
    var value = e.detail.value, len = parseInt(value.length);
    if (len > this.data.noteMaxLen) return;

    this.setData({
      currentNoteLen: len //当前字数  
      //limitNoteLen: this.data.noteMaxLen - len //剩余字数  
    });
  }
  }
};
</script>

<style>
/**index.wxss**/
.title{
  width: 90%;
  height: 30px;
  margin:10px auto;
  font-weight: bold;
  font-family: "微软雅黑";
  font-size: 16px;
}

.int{
  width: 90%;
  margin:10px auto;
  font-size: 14px;
  font-family: "微软雅黑";
  color: #666;
}

.textarea{
  width: 80%;
  height: 60px;
  border:none;
  margin:10px auto 0;
  background: #fff;
  padding: 15px 15px;
  font-size: 15px;
}

.phcolor{
    color:#7C7C7C;
    margin: 10px 10px;
    font-size: 15px;
    font-family: "微软雅黑";
}

.ff-hv{
  width: 90%;
  height: 25px;
  background: #fff;
  display: block;
  margin:0 auto;
  color: #C9C9C9;
  font-size: 15px;
  text-align: right;
  border-right:10px solid #fff;
  box-sizing: border-box;
}

.imgfile{
  width: 90%;
  height: 20px;
  margin: 10px auto;
  font-family: "微软雅黑";
  font-size: 15px;
}

.imgselect{
  width: 80px;
  height: 80px;
  border:1px solid #DCDCDC;
  text-align: center;
  line-height: 80px;
  font-size: 100px;
  font-weight: light;
  background: url(http://wxpad.cn/yunpan/cdn/imgsrc/1523702768.png);
}

.imgselect image{
  width: 80px;
  height: 80px;
}

.btn{
  width:80%;
  margin:0 auto;
  background: #20AC19;
  color:#fff;
}

.kefu{
  width: 20px;
  height: 20px;
  display: block;
  margin:10px auto;
}

.footer{
  width: 125px;
  height: 30px;
  margin:10px auto;
}

.footer image{
  width: 125px;
  height: 30px;
}

.xgurl{
  width: 90%;
  height: 45px;
  margin:10px auto;
  background: #fff;
}

.xgurl-phcolor{
  color:#7C7C7C;
  font-size: 15px;
  text-indent:10px;
  font-family: "微软雅黑";
}	
</style>
