<template>
  <view class="container">
    <!-- 表格 -->
    <view class="table">
      <!-- 表头 -->
      <view class="table-header">
        <text class="header-cell">身高 (cm)</text>
        <text class="header-cell">体重 (kg)</text>
        <text class="header-cell">BMI</text>
      </view>
      <!-- 数据行 -->
      <view v-for="(item, index) in dataList" :key="index" class="table-row">
        <text class="cell">{{ item.height }}</text>
        <text class="cell">{{ item.weight }}</text>
        <text class="cell">{{ item.bmi.toFixed(2) }}</text>
      </view>
    </view>
  </view>
</template>

<script>
	export default{
		data(){
			return{
				dataList: []
			}
		},
		onLoad() {
			
		},
		methods:{
			    fetchData() {
			      // 模拟 API 请求
			      // 实际应用中应替换为真实的 API 请求代码
			      uni.request({
			        url: 'https://example.com/api/bmi-data', // 替换为真实 API 地址
			        method: 'GET',
			        success: (response) => {
			            this.dataList = response.data.map(item => {
			              // 假设 API 返回的数据格式是 { height: number, weight: number }
			              const height = item.height;
			              const weight = item.weight;
			              const heightInMeters = height / 100;
			              const bmi = weight / (heightInMeters * heightInMeters);
			              return { height, weight, bmi };
			            });
			        },
			        fail: (error) => {
			          uni.showToast({
			            title: '请求失败',
			            icon: 'none'
			          });
			        }
			      });
			    }
			  
		}
	}
</script>

<style>
	.container {
	  padding: 20px;
	}
	
	.table {
	  border-collapse: collapse;
	  width: 100%;
	}
	
	.table-header, .table-row {
	  display: flex;
	  justify-content: space-between;
	  padding: 10px;
	  border-bottom: 1px solid #ccc;
	}
	
	.table-header {
	  background-color: #f0f0f0;
	  font-weight: bold;
	}
	
	.table-row {
	  background-color: #fff;
	}
	
	.header-cell, .cell {
	  flex: 1;
	  text-align: center;
	}
	
	.header-cell {
	  font-size: 16px;
	}
	
	.cell {
	  font-size: 14px;
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
</style>