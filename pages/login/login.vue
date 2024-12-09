<template>
	<view class="zai-box">
		<image src="../../static/609a42e626608.jpg" mode='aspectFit' class="zai-logo"></image>
		<view class="zai-title"></view>
		<view class="zai-form">
			<input class="zai-input" placeholder-class placeholder="请输入用户名" v-model="name" />
			<input class="zai-input" placeholder-class password placeholder="请输入密码" v-model="pwd"/>
			<navigator @click="login0" hover-class="none" class="zai-label">免登录</navigator>
			<button class="zai-btn" @click="sendRequest">立即登录</button>
			<navigator url="../login/register" hover-class="none" class="zai-label">还没有账号？点此注册.</navigator>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
			  name: "",
			  pwd:"",
			}
		},
  onLoad() {

  },
methods: {
	 login0(){
		 uni.switchTab({
		 	url:'/pages/myinfo/myinfo'
		 })
	 },
  sendRequest() {
   uni.$emit('pwd',this.pwd)
    uni.$emit('name',this.name)
	const app = getApp();
	        app.globalData.pwd = this.pwd;
	        app.globalData.name = this.name;
			
    uni.showLoading({
      title: "正在登陆..."
    }),
    uni.request({
      url:'http://localhost:7070/user/login/'+this.name+'/'+this.pwd,
      timeout: 6000,
	  success: (res) => {
	  	// uni.setStorage({
	  	// 	  	key:pwd,
	  	// 	  	data:this.pwd,
	  	// 	  }),
	  	// 	  uni.setStorage({
	  	// 	  	key:name,
	  	// 	  			data:this.name,
	  	// 	 })
			 uni.showToast({
			   title: '登录成功',
			   icon: 'success',
			   mask: true,
			   duration: 3000 ,// 设置显示时长
			 });
			 uni.reLaunch({
			 	url: '/pages/myinfo/myinfo?name='+this.name+'&pwd='+this.pwd
			 })
			 uni.hideLoading();
	  },
	  fail: (res) => {
	  	uni.showToast({
	  	  title: '登录失败',
	  	  icon: 'fail',
	  	  mask: true,
	  	  duration: 3000 ,// 设置显示时长
	  	});
		 uni.hideLoading();
	  }
    });
  }
}
}
</script>

<style>
	.zai-box{
		padding: 0 100upx;
		position: relative;
	}
	.zai-logo{
		width: 100%;
		width: 100%;
		height: 310upx;
	}
	.zai-title{
		position: absolute;
		top: 0;
		line-height: 360upx;
		font-size: 68upx;
		color: #fff;
		text-align: center;
		width: 100%;
		margin-left: -100upx;
	}
	.zai-form{
		margin-top: 300upx;
	}
	.zai-input{
		background: #e2f5fc;
		margin-top: 30upx;
		border-radius: 100upx;
		padding: 20upx 40upx;
		font-size: 36upx;
	}
	.input-placeholder, .zai-input{
		color: #94afce;
	}
	.zai-label{
		padding: 60upx 0;
		text-align: center;
		font-size: 30upx;
		color: #a7b6d0;
	}
	.zai-btn{
		background: #ff65a3;
		color: #fff;
		border: 0;
		border-radius: 100upx;
		font-size: 36upx;
	}
	.zai-btn:after{
		border: 0;
	}
	/*按钮点击效果*/
	.zai-btn.button-hover{
		transform: translate(1upx, 1upx);
	}
</style>
