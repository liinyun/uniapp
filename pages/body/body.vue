<template>
	<view>
 <view class="card">
  	<view class="menu">
		<view>胸围:{{BustSize}}</view>
  		<view class="item" >
  			<view>
  				<horizontal-rule :min="10" :max="50" :value="20"  @change="change1" multiple="10"  v-model="BustSize" ></horizontal-rule>
  					 <vertical-rule :min="10" :max="50" :value="20" @change="change1" multiple="10"></vertical-rule>
  				</view>
  	</view>
  </view>
  </view>
  <view class="card">
  	<view class="menu">
		<view>下胸围:{{UnderbustSize}}</view>
  		<view class="item" >
  			<horizontal-rule :min="0" :max="50" :value="20" @change="change2"  v-model="UnderbustSize" multiple="10"></horizontal-rule>
  				 <vertical-rule :min="0" :max="50"  @change="change2" multiple="10" ></vertical-rule>
  			</view>
  	</view>
  </view>
  <view class="card">
  	<view class="menu">
		<view>两乳头点间距:{{DistanceBetweenNipples}}</view>
  		<view class="item" >
  			<horizontal-rule :min="10" :max="50" :value="20" @change="change3" v-model="DistanceBetweenNipples" multiple="10"></horizontal-rule>
  				 <vertical-rule :min="10" :max="50" :value="20" @change="change3" multiple="10"></vertical-rule>
  			</view>
  	</view>
  </view>
  <view class="card">
  	<view class="menu">
		<view>体表弧长（下缘）:{{BPBodyArcLengthLowerEdge}}</view>
  		<view class="item" >
  			<horizontal-rule :min="10" :max="50" :value="20" @change="change4" v-model="BPBodyArcLengthLowerEdge" multiple="10"></horizontal-rule>
  				 <vertical-rule :min="10" :max="50" :value="20" @change="change4" multiple="10"></vertical-rule>
  			</view>
  	</view>
  </view>
  <view class="card">
  	<view class="menu">
		<view>体表弧长（内缘）:{{BPBodyArcLengthInnerEdge}}</view>
  		<view class="item" >
  			<horizontal-rule :min="10" :max="50" :value="20" @change="change5" v-model="BPBodyArcLengthInnerEdge" multiple="10"></horizontal-rule>
  				 <vertical-rule :min="10" :max="50" :value="20" @change="change5" multiple="10"></vertical-rule>
  			</view>
  	</view>
  </view>
  <view class="card">
  	<view class="menu">
		<view>体表弧长（外缘）:{{BPBodyArcLengthOuterEdge}}</view>
  		<view class="item" >
  			<horizontal-rule :min="10" :max="50" :value="20" @change="change6" v-model="BPBodyArcLengthOuterEdge" multiple="10"></horizontal-rule>
  				 <vertical-rule :min="10" :max="50" :value="20" @change="change6" multiple="10"></vertical-rule>
  			</view>
  	</view>
  </view>
  <view class="card">
  	<view class="menu">
		<view>右乳体积（立方厘米）:{{RightBreastVolumeCubicCm}}</view>
  		<view class="item" >
  			<horizontal-rule :min="10" :max="50" :value="20" @change="change7" v-model="RightBreastVolumeCubicCm" multiple="10"></horizontal-rule>
  				 <vertical-rule :min="10" :max="50" :value="20" @change="change7" multiple="10"></vertical-rule>
  			</view>
  	</view>
  </view>
  <view >
	  <button @click="sendInfo"  style="background: #ff65a3;
		color: #fff;
		border: 0;
		width: 50%;
		border-radius: 100upx;
		font-size: 36upx;">确定</button>
  </view>
 </view>
	<!-- <view>
		<text style="font-weight: bold;font-size: 15px;">使用说明:</text><br>
		<text class="text">1.测量姿势为站姿。</text><br>
		<text class="text">2.下垂胸型的胸部细节尺寸穿着单层无衬垫无支撑物胸罩测量。</text><br>
		<text class="text">3.预测文胸尺码范围为70B-95F,腰背夹尺码范围为70-98，束裤尺码范园为70-106。</text>
	</view> -->

</template>


<script>
	export default {
		data(){
			return{
				pwd:"",
				name:"",
				BustSize:0,
				UnderbustSize:'',
				DistanceBetweenNipples:'',
				BPBodyArcLengthLowerEdge:'',
				BPBodyArcLengthInnerEdge:'',
				BPBodyArcLengthOuterEdge:'',
				RightBreastVolumeCubicCm:''
			}
		},
		onLoad() {
			 const app = getApp();
			    
			    // 如果全局变量已经存在，直接获取
			    if (app.globalData.pwd) {
			        this.pwd = app.globalData.pwd;
			    }
			    if (app.globalData.name) {
			        this.name = app.globalData.name;
			    }
			// 监听 'pwd' 事件
			    uni.$on('pwd', (res) => {
			        this.pwd = res; // 将接收到的 pwd 数据赋值给本地变量
					
			    })
			
			    // 监听 'name' 事件
			    uni.$on('name', (res) => {
			        this.name = res; // 将接收到的 name 数据赋值给本地变量
			    })
				},
		onUnload() {
					
				},
		methods: {
			sendInfo(){
			    uni.request({
					method:'POST',
			        url: "http://127.0.0.1:7070/body/save/"+this.pwd+"/"+this.name+"/"+this.BustSize+"/"+this.UnderbustSize+"/"+this.DistanceBetweenNipples+"/"+this.BPBodyArcLengthLowerEdge+"/"
					+this.BPBodyArcLengthInnerEdge+"/"+this.BPBodyArcLengthOuterEdge+"/"+this.RightBreastVolumeCubicCm+"/",
			        success: (res) => {
						console.log(res.data)
			            uni.reLaunch({ // 跳转推荐页面
			                url: '/pages/recommend/recommend'
			            });
			        },
			    });
			},

			// 子组件抛出的选择结果
			change1: function(e) {
			    this.BustSize = Math.round(e*10)/10
			},
			change2: function(e) {
			    this.UnderbustSize = Math.round(e*10)/10
			},
			change3: function(e) {
			    this.DistanceBetweenNipples = Math.round(e*10)/10
			},
			change4: function(e) {
			    this.BPBodyArcLengthLowerEdge = Math.round(e*10)/10
			},
			change5: function(e) {
			    this.BPBodyArcLengthInnerEdge = Math.round(e*10)/10
			},
			change6: function(e) {
			    this.BPBodyArcLengthOuterEdge = Math.round(e*10)/10
			},
			change7: function(e) {
			    this.RightBreastVolumeCubicCm = Math.round(e*10)/10
			},
		submitForm() {//表单提交，发送请求，同时跳转页面,测量示意图-detail界面，最后展示推荐结果
		    // 将表单数据注入到 formData 中
		    this.formData = {
		   		            BMI: this.formData.BMI,
		   		            BC: this.formData.BC,
		   		            UBC: this.formData.UBC,
		   		            WC: this.formData.WC,
		   		            AC: this.formData.AC,
		   		            HC: this.formData.HC,
		   		            RBP_LBP: this.formData.RBP_LBP,
		   		            RBP_LEP: this.formData.RBP_LEP,
		   		            IEP_RBP_OEP: this.formData.IEP_RBP_OEP,
		   		        };
		    console.log(this.formData); // 打印 formData，检查是否正确注入数据
			
			
			// const requestTask = uni.request({//发送请求，向后端请求数据
			// 	url: 'https://www.baidu.com', //仅为示例，并非真实接口地址。
			// 	data: {//附带参数，正常?id=x,不是restful请求
			      
			// 	},
			// 	success: function(res) {
			// 		console.log(res.data);//打印
			// 	}
			// });
			//跳转到detail界面
			uni.navigateTo({
				url:'/pages/detail/detail'
			})
		},
		}
	}
</script>

<style scoped lang="scss">

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
</style>