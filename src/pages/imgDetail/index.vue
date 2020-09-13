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
    console.log(this.imgDetail)
  }
}
</script>

<style lang="scss">
</style>
