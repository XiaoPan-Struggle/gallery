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

  <!-- 点赞，收藏 -->
  <view class="user_rank">
    <view class="rank">
      <text class="iconfont icondianzan"> {{imgDetail.rank}} </text>
    </view>
    <view class="user_collect">
      <text class="iconfont iconshoucang"> 收藏 </text>
    </view>
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
// 作者
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

// 点赞
.user_rank {
  display: flex;
  text-align: center;
  border-bottom: 5rpx solid #f9f9f9;

  .rank {
    flex: 1;
    padding: 20rpx 0;

    text.icondianzan {}
  }

  .user_collect {
    flex: 1;
    padding: 20rpx 0;

    text.iconshoucang {}
  }
}
</style>
