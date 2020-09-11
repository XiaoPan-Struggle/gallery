<template>
<view>
  <!-- 推荐 -->
  <view class="recommend_warp">
    <view class="recommend_item" v-for="(item) in recommends" :key="item.id">
      <image mode="widthFix" :src="item.thumb"></image>
    </view>
  </view>
</view>
</template>

<script>
export default {
  data() {
    return {
      recommends: []
    }
  },
  mounted() {
    this.request({
        url: 'http://157.122.54.189:9088/image/v3/homepage/vertical',
        data: {
          // 获取多少条数据
          limit: 30,
          // 关键字
          order: 'hot',
          // 跳过几条
          skip: 0
        }
      })
      .then(result => {
        this.recommends = result.res.homepage[1].items;
      })
  }
}
</script>

<style lang="scss">
.recommend_warp {
  display: flex;
  flex-wrap: wrap;

  .recommend_item {
    width: 50%;
    border: 5rpx solid #fff;
  }
}
</style>
