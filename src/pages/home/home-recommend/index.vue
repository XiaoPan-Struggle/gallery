<template>
<view v-if="recommends.length > 0">
  <!-- 推荐 -->
  <view class="recommend_warp">
    <view class="recommend_item" v-for="item in recommends" :key="item.id">
      <image mode="widthFix" :src="item.thumb"></image>
    </view>
  </view>

  <!-- 月份 -->
  <view class="monthes_warp">
    <view class="monthes_title">
      <view class="monthes_title_info">
        <view class="monthes_info">
          <text>{{ months.DD }}</text>
          / {{ months.MM }} 月
        </view>
        <view class="monthes_text">{{ months.title }}</view>
      </view>
      <view class="monthes_title_more">更多 ></view>
    </view>
    <view class="monthes_content">
      <view class="monthes_item" v-for="item in months.items" :key="item.id">
        <image mode="aspectFill" :src="item.thumb + item.rule.replace('$<Height>', 360)"></image>
      </view>
    </view>
  </view>
</view>
</template>

<script>
import moment from 'moment'
export default {
  data() {
    return {
      // 推荐
      recommends: [],
      // 月份
      months: {},
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
        skip: 0,
      },
    }).then((result) => {
      // 推荐
      this.recommends = result.res.homepage[1].items
      // 月份
      this.months = result.res.homepage[2]
      // 月
      this.months.MM = moment(this.months.stime).format('MM')
      this.months.DD = moment(this.months.stime).format('DD')
      console.log(result)
    })
  },
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
.monthes_warp {
  .monthes_title {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 20rpx;

    .monthes_title_info {
      display: flex;
      color: $color;
      font-size: 30rpx;
      font-weight: 600;

      .monthes_info {
        text {
          font-size: 36rpx;
        }
      }

      .monthes_text {
        margin-left: 30rpx;
        color: #5b5b5b;
        font-size: 34rpx;
      }
    }

    .monthes_title_more {
      color: $color;
      font-size: 24rpx;
      font-weight: 600;
    }
  }

  .monthes_content {
    display: flex;
    flex-wrap: wrap;

    .monthes_item {
      width: 33.3%;
      border: 5rpx solid #fff;
    }
  }
}
</style>
