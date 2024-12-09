<template>
	  <uni-easyinput placeholder="请输入原密码" v-model="oldpwd"></uni-easyinput>
	  <uni-easyinput placeholder="请输入新密码"  v-model="newpwd"></uni-easyinput>
	  <uni-easyinput placeholder="请再次输入新密码" v-model="newpwd2"></uni-easyinput>
	  <button @click="updatepwd">确定</button>
</template>

<script>
	export default{
		data(){
			return{
				oldpwd:'',
				newpwd:'',
				newpwd2:'',
				name:'',
				pwd:''
			}
		},
		onLoad(options) {
			this.name=options.name
			this.pwd=options.pwd
		},
		methods:{
			updatepwd(){
				if(this.oldpwd!=this.pwd){
					uni.showModal({
						content:'原密码错误'
					})
				}
				if(this.newpwd!=this.newpwd2){
					uni.showModal({
						content:'两次密码不一致'
					})
				}
				uni.request({
					url:'http://localhost:7070/user/change/'+this.name+'/'+this.newpwd+'/'+this.oldpwd,
					success: (res) => {
						uni.showModal({
							content:'修改成功'
						})
						uni.switchTab({
							url:'/pages/myinfo/myinfo'
						})
					},
					fail: (res) => {
						uni.showModal({
							content:'修改失败'
						})
					}
				})
			}
		}
	}
</script>

<style>
	
</style>