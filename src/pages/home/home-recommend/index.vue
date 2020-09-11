<template>
<view>
  <!-- 推荐 -->
  <view class="recommend_warp">
    <view class="recommend_item" v-for="(item) in recommends" :key="item.id">
      <image mode="widthFix" :src="item.thumb"></image>
    </view>
  </view>

  <!-- 月份 -->
  <view class="moneths_warp">
    <view class="moneths_title">
      <view class="moneths_title_info">
        <view class="moneths_info">
          <text>18</text>
          / 01 月
        </view>
        <view class="moneths_text">你负责美丽就好</view>
      </view>
      <view class="moneths_title_more">更多 ></view>
    </view>
    <view class="moneths_content"></view>
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
// 推荐
.recommend_warp {
  display: flex;
  flex-wrap: wrap;

  .recommend_item {
    width: 50%;
    border: 5rpx solid #fff;
  }
}

// 月份
.moneths_warp {
  .moneths_title {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 20rpx;

    .moneths_title_info {
      display: flex;
      color: $color;
      font-size: 30rpx;
      font-weight: 600;

      .moneths_info {
        text {
          font-size: 36rpx;
        }
      }

      .moneths_text {
        margin-left: 30rpx;
        color: #5b5b5b;
        font-size: 34rpx;
      }
    }

    .moneths_title_more {
      color: $color;
      font-size: 24rpx;
      font-weight: 600;
    }
  }

  .moneths_content {}
}
</style>
