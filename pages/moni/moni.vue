<template>
  <view class="container">
    <view class="card">
      <view class="header">
        <text class="title">虚拟试衣间</text>
      </view>
      <form @submit.prevent="uploadImage" class="upload-form">
        <input type="file" @change="onFileChange" accept="image/*" required class="file-input" />
        <button type="submit" class="upload-button">上传图片</button>
      </form>
      <view  class="result">
        <text class="result-title">试衣效果:</text>
        <image :src="imageUrl" mode="aspectFit" class="result-image" />
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      selectedFile: null,
      imageUrl: ''
    };
  },
  methods: {
    onFileChange(event) {
      this.selectedFile = event.target.files[0];
    },
    async uploadImage() {
      const formData = new FormData();
      formData.append('image', this.selectedFile);

      try {
        const response = await uni.request({
          url: 'http://your-django-server/upload/', // 替换为您的 Django 服务器 URL
          method: 'POST',
          data: formData,
          header: {
            'Content-Type': 'multipart/form-data'
          }
        });

        if (response.data.image_url) {
          this.imageUrl = response.data.image_url; // 显示处理后的试衣图片
        } else {
          console.error(response.data.error);
        }
      } catch (error) {
        console.error('Error:', error);
      }
    }
  }
};
</script>

<style scoped lang="scss">
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background-color: #ffeef8; /* 浅粉色背景 */
}

.card {
  background: #fff;
  box-sizing: border-box;
  width: 700rpx;
  margin: auto;
  padding: 15rpx;
  margin-top: 20rpx;
  border-radius: 12rpx;

  .header {
    text-align: center;
    margin-bottom: 20rpx;

    .title {
      font-size: 24px;
      font-weight: bold;
      color: #d5006d; /* 深粉色 */
    }
  }

  .upload-form {
    display: flex;
    flex-direction: column;
    align-items: center;

    .file-input {
      margin-bottom: 10rpx;
      padding: 10rpx;
      border: 2px solid #d5006d; /* 深粉色边框 */
      border-radius: 5rpx;
      background-color: #fff; /* 白色背景 */
      width: 100%;
    }

    .upload-button {
      padding: 10rpx 20rpx;
      background-color: #d5006d; /* 深粉色按钮 */
      color: #fff; /* 白色文字 */
      border: none;
      border-radius: 5rpx;
      cursor: pointer;
      font-size: 16px;
      width: 100%;
    }

    .upload-button:hover {
      background-color: #b0004d; /* 按钮悬停时的颜色 */
    }
  }

  .result {
    margin-top: 20rpx;
    text-align: center;

    .result-title {
      font-size: 20px;
      color: #ff65a3; /* 深粉色 */
      margin-bottom: 10rpx;
    }

    .result-image {
      width: 100%;
      height: auto;
      border: 2px solid #ff65a3; /* 深粉色边框 */
      border-radius: 10rpx;
    }
  }
}
</style>