<template>
<view class="video_play">
  <image :src="videoObj.img"></image>
  <!-- 顶部工具栏 -->
  <view class="video_tool">
    <view class="iconfont iconshengyin"></view>
    <view class="iconfont iconzhuanfa"></view>
  </view>
  <!-- 视频 开始 -->
  <view class="video_wrap">
    <video :src="videoObj.video" objectFit="fill"></video>
  </view>
  <!-- 下载  开始 -->
  <view class="download">
    <view class="download_btn">下载高清</view>
  </view>
</view>
</template>

<script>
export default {
  data() {
    return {
      videoObj: {}
    }
  },
  onLoad() {
    console.log(getApp().globalData, '....')
    this.videoObj = getApp().globalData.video
  }
}
</script>

<style lang="scss">
.video_play {
  position: relative;

  image {
    position: absolute;
    width: 100vw;
    height: 100vh;
    z-index: -1;
    // c3 滤镜
    filter: blur(20px);
  }

  .video_tool {
    height: 80rpx;
    display: flex;
    justify-content: flex-end;

    .iconfont {
      width: 80rpx;
      color: #fff;
      font-size: 50rpx;
      border-radius: 40rpx;
      background-color: rgba(0, 0, 0, .2);
      display: flex;
      justify-content: center;
      align-items: center;
      margin-right: 20rpx;
    }

    .iconfont.iconzhuanfa {}
  }

  .video_wrap {
    display: flex;
    justify-content: center;

    video {
      width: 360rpx;
      height: 600rpx;
    }
  }

  .download {
    display: flex;
    justify-content: center;

    .download_btn {
      width: 360rpx;
      height: 80rpx;
      margin-top: 40rpx;
      border-radius: 40rpx;
      display: flex;
      justify-content: center;
      align-items: center;
      color: #fff;
      border: 3px solid #fff;
      background-color: rgba(0, 0, 0, .6);
    }
  }
}
</style>
