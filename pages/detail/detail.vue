<template>
<view class="container">
	
</view>






<button style="width: 30%; height: 30px; margin-left: 40%; text-align: center; line-height: 30px; vertical-align: middle;
margin-top:100%;" @click="goToRecommend()">
	查看推荐</button>
</template>

<script>
	export default {
 		data(){
			return{
				
			}
		},
		methods:{
			goToRecommend(){
				uni.navigateTo({
					url:'/pages/recommend/recommend'
				})
			}
		}
	}
</script>

<style scoped>
.container{
	
	background-size: cover;
	   background-position: center;
	   background-attachment: fixed;
	   height: 100vh; /* 确保容器高度占据整个视口 */
	   overflow: hidden;
}
</style>