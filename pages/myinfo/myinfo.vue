<template>
	<view class="page">
		<view class="user">
			
			<image class="avatar" @click="chooseimage" :src="img" > </image>
			<view class="name">
				<text @click="login">{{name}}</text>
			</view>
			<image class="right" src="/static/center4/right.png"></image>
		</view>
		<view class="tip"> 
		
			<view @click="ShowHeight">身高:{{hight}}</view>
			<view @click="ShowWeight">体重:{{weight}}</view>
			<view>
				<text >锻炼天数</text>
				<text>{{day}}</text>
			</view>
		</view>
		<view class="card">
			<view class="menu">
				<view class="item" >
					<view  @click="changeday">今日打卡(上次打卡:{{lasttime}})</view>
				</view>
			</view>
		</view>
		<view class="card">
			<view class="menu">
				<view class="item" >
					<view @click="countBMI">BMI:{{BMI}}(重新计算){{msg}}</view>
				</view>
			</view>
		</view>
		<view class="card">
			<view class="menu">
				<view class="item" >
					<view @click="bodychange">我的变化</view>
					<image class="arrow" ></image>
				</view>
			</view>
		</view>
		<view class="card">
			<view class="menu">
				<view class="item" >
					<view @click="changepwd">修改密码</view>
				</view>
			</view>
		</view>
		
		<view class="card">
			<view class="menu">
				<view class="item" >
					<view @click="manager">联系管理员</view>
					<image class="arrow" ></image>
				</view>
			</view>
		</view>
		<view class="card">
			<view class="menu">
				<view class="item" >
					<view @click="out">退出登录</view>
					<image class="arrow" ></image>
				</view>
			</view>
		</view>
		
	</view>
</template>


<script>
	export default {
		data() {
			return {
				img:'/static/609a42e626608.jpg',
				name:'未登录',
				hight:'',
				weight:'',
				BMI:'',
				day:'',
				timeok:true,
				lasttime:'2024年1月1日',
				msg:'完美身材！继续保持！',
				pwd:''
			}
		},

		onLoad(options) {
			this.name=options.name
			this.pwd=options.pwd
			uni.request({
				method:'GET',
				url:'http://localhost:7070/bmi/'+this.name+'/'+this.pwd,
				success: (res) => {
					this.hight=res.data.data.height
					this.BMI=res.data.data.bmi
					this.weight=res.data.data.weight
					this.day=res.data.data.day
				}
			})
			 this.imageList = []
			  const bmi=Number(this.BMI)
			console.log('0000')
			      if (bmi < 18.5) {
			        this.msg = '体重过轻'
			      } else if (bmi >= 18.5 && bmi < 24.9) {
			        this.msg = '正常范围'
			      } else if (bmi >= 25 && bmi < 29.9) {
			        this.msg = '体重过重'
			      } else {
			        this.msg = '肥胖'
			      }
			    
		
		},
		methods: {
		
			chooseimage(){
				uni.chooseImage({
					success: (res) => {
						this.img=res.tempFilePaths[0]
					},
					fail: (res) => {
						uni.showModal({
							content:'请重试'
						})
					}
				})
			},
       bodychange(){
		  uni.navigateTo({
		  	url:'/pages/recommend/BMI'
		  })  
	   },
			out(){
			  uni.navigateTo({
			  	url:'/pages/login/login'
			  })	
			},
			manager(){
			   uni.showModal({
			   	cancelColor:"pink",
				content:"请给xxxxxxxx.com发送邮件",
			   })	
			},
			changepwd(){ 
               uni.navigateTo({
               	url:'/pages/myinfo/changgepwd?name='+this.name+'&pwd='+this.pwd
               })
			},
			changeday(){
				if(this.timeok){
					this.day=Number(this.day)+1,
					uni.request({
						method:'GET',
						url:'http://localhost:7070/bmi/'+this.day+'/'+this.hight+'/'+this.weight+'/'+this.pwd+'/'+this.name,
						complete: () => {
							uni.showModal({
								confirmColor:'pink',
								content:'坚持做更好的自己！',
								title:'锻炼打卡',
							})		
						}
					})
				}
	    uni.request({
	    	method:'GET',
	    	url:'http://localhost:7070/bmi/'+this.name+'/'+this.pwd,
	    	success: (res) => {
	    		this.hight=res.data.data.height
	    		this.BMI=res.data.data.bmi
	    		this.weight=res.data.data.weight
	    		this.day=res.data.data.day
	    	}
	    })
			},
	
			login(){
			 uni.switchTab({
			 	url:'/pages/login/login'
			 })	
			},
          ShowHeight(){
			  uni.showModal({
			  	placeholderText:'请输入身高(cm)',
				editable:true,
				success: (res) => {
					this.hight=res.content,
					uni.request({
						url:'http://localhost:7070/bmi/'+this.day+'/'+this.hight+'/'+this.weight+'/'+this.pwd+'/'+this.name,//更新身高
						success() {
							uni.showModal({
								content:'修改成功,请打卡'
							})
						},
						fail() {
							uni.showModal({
								content:'修改失败'
							})
						}
					})
				}
			  })
		  },
		  ShowWeight(){
			  uni.showModal({
			  	placeholderText:'请输入体重(kg)',
			  				editable:true,
			  				success: (res) => {
			  					this.weight=res.content,
								uni.request({
									url:'http://localhost:7070/bmi/'+this.day+'/'+this.hight+'/'+this.weight+'/'+this.pwd+'/'+this.name,//更新体重
									success() {
										uni.showModal({
											content:'修改成功，请打卡'
										})
									},
									fail() {
										uni.showModal({
											content:'修改失败'
										})
									}
								})
			  				}
			  })
		  }
		}
	}
</script>

<style lang="scss">
	.page {
		background: #f5f5f5;
		min-height: 100vh;
		

		
		.user{
			display: flex;
			align-items: center;
			padding: 25rpx;
			background: #fff;
			
			&:active{
				background: #f0f0f0;
			}
			
			.avatar{
				width: 90rpx;
				height: 90rpx;
				border-radius: 50%;
			}
			.right{
				width: 30rpx;
				height: 30rpx;
			}
			.name{
				flex-grow: 1;
				display: flex;
				flex-direction: column;
				padding-left: 20rpx;
				
				text{
					&:nth-child(1){
						font-size: 16px;
						font-weight: bold;
					}
					&:nth-child(2){
						font-size: 12px;
						color: #777;
					}
				}
			}
		}
		
		.tip{
			background: pink;
			width: 700rpx;
			margin: auto;
			height: 100rpx;
			border-radius: 12rpx;
			margin-top: 20rpx;
			display: flex;
			justify-content: space-between;
			align-items: center;
			color: #fff;
			
			view{
				padding-left: 20rpx;
				width: 330rpx;
				
				text{
					font-size: 14px;
					&:nth-child(2){
						font-size: 16px;
						font-weight: bold;
						margin-left: 10rpx;
					}
				}
			}
		}
		
		.card{
			background: #fff;
			box-sizing: border-box;
			width: 700rpx;
			margin: auto;
			padding: 15rpx;
			margin-top: 20rpx;
			border-radius: 12rpx;
			
			.menu{
				
				.item{
					display: flex;
					height: 100rpx;
					align-items: center;
					padding: 0 20rpx;
					box-sizing: border-box;
					width: 700rpx;
					
					&:active{
						background: #f0f0f0;
					}
					
					image{
						height: 45rpx;
						width: 45rpx;
					}
					
					view{
						font-size: 14px;
						color: #444;
						padding-left: 15rpx;
						box-sizing: border-box;
						width: calc(100% - 100rpx);
					}
					
					.arrow{
						width: 26rpx;
						height: 26rpx;
					}
				}
			}
		}
		
	}
</style>