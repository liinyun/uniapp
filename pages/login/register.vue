<template>
	<view class="zai-box">
		<image src="../../static/609a42e626608.jpg" mode='aspectFit' class="zai-logo"></image>
		<view class="zai-title"></view>
		<view class="zai-form">
			<input class="zai-input" placeholder-class placeholder="请输入用户名" v-model="name"/>
			<input class="zai-input" placeholder-class password placeholder="请输入密码" v-model="pwd"/>
			<input class="zai-input" placeholder-class password placeholder="请再输入一次密码" v-model="pwdre"/>
			<button class="zai-btn" @click="register">立即注册</button>
			<navigator url="../login/login" open-type='navigateBack' hover-class="none" class="zai-label">已有账号，点此去登录.</navigator>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				pwd:"",
				pwdre:"",
				name:"",
			}
		},
		methods: {
			register(){
				if(this.pwd!=this.pwdre){
					uni.showModal({
						content:'两次密码不一致'
					})
				}
				uni.request({
			        method:'POST',
					url:'http://localhost:7070/user/register/'+this.name+'/'+this.pwd,
					complete: () => {
						uni.showModal({
							content:'注册成功'
						}),
						uni.switchTab({
							url:'/pages/login/login'
						})
					},
				})
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
		margin-top: 60upx;
	}
	.zai-btn:after{
		border: 0;
	}
	/*按钮点击效果*/
	.zai-btn.button-hover{
		transform: translate(1upx, 1upx);
	}
</style>
s
