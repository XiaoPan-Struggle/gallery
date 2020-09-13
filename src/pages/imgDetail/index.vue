<template>
<view>

  <!-- 作者 -->
  <view class="user_info">
    <view class="user_icon">
      <image :src="imgDetail.user.avatar" mode="widthFix"></image>
    </view>
    <view class="user_desc">
      <view class="user_name">{{imgDetail.user.name}}</view>
      <view class="user_time">{{imgDetail.atime}}</view>
    </view>
  </view>

  <!-- 发布的大图 -->
  <view class="high_img">
    <image mode="widthFix" :src="imgDetail.newThumb"></image>
  </view>

</view>
</template>

<script>
export default {
  data() {
    return {
      imgDetail: {}
    }
  },
  onLoad() {
    const {
      imgIndex,
      imgList
    } = getApp().globalData;
    this.imgDetail = imgList[imgIndex]
    // 直接在路径上写入会报错，因为还没有拿到，路径会是undefined
    this.imgDetail.newThumb = this.imgDetail.thumb + this.imgDetail.rule.replace('$<Height>', 360)
    console.log(this.imgDetail)
  }
}
</script>

<style lang="scss" scoped>
.user_info {
  display: flex;
  padding: 30rpx;

  .user_icon {
    border-radius: 44rpx;
    overflow: hidden;

    image {
      width: 88rpx;
    }
  }

  .user_desc {
    display: flex;
    flex-direction: column;
    justify-content: center;
    padding-left: 30rpx;
    font-size: 26rpx;

    .user_name {
      font-weight: 600;
    }

    .user_time {
      color: #d3d3d3;
    }
  }
}
</style>
