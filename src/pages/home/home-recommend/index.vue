<template>
<scroll-view class="recommends_view" @scrolltolower="handleToLower" scroll-y v-if="recommends.length > 0">
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

  <!-- 热门 -->
  <view class="hots_warp">
    <view class="hots_title">
      <text> 热门 </text>
    </view>
    <view class="hots_content">
      <view class="hot_item" v-for="item in verticals" :key="item.id">
        <image mode="widthFix" :src="item.thumb"></image>
      </view>
    </view>
  </view>
</scroll-view>
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
      // 热门
      verticals: [],
      // 请求参数
      params: {
        // 获取多少条数据
        limit: 30,
        // 关键字
        order: 'hot',
        // 跳过几条
        skip: 0,
      },
      // 有无下一页
      hasMore: true,
    }
  },
  mounted() {
    this.getList()
  },
  methods: {
    // 获取接口数据
    getList() {
      this.request({
        url: 'http://157.122.54.189:9088/image/v3/homepage/vertical',
        data: this.params,
      }).then((result) => {
        // 第一次请求才触发
        if (this.recommends.length === 0) {
          // 推荐
          this.recommends = result.res.homepage[1].items
          // 月份
          this.months = result.res.homepage[2]
          // 月
          this.months.MM = moment(this.months.stime).format('MM')
          this.months.DD = moment(this.months.stime).format('DD')
        }
        // 判断有无下一页
        if (result.res.vertical.length === 0) {
          this.hasMore = false
          return
        }
        // 热门  数据拼接
        this.verticals = [...this.verticals, ...result.res.vertical]
        console.log(result)
      })
    },
    // 滚动条触底事件
    handleToLower() {
      /* 
        1. 修改参数 skip += limit
        2. 发送请求
        3. 数据叠加 hots
       */
      if (this.hasMore) {
        this.params.skip += this.params.limit
        this.getList()
      } else {
        uni.showToast({
          title: '到底啦!',
          icon: 'none',
        })
      }
    },
  },
}
</script>

<style lang="scss">
.recommends_view {
  // 视口-头部tab高
  height: calc(100vh - 36px);
}

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

// 热门
.hots_warp {
  .hots_title {
    padding: 20rpx;

    text {
      padding: 0 10rpx;
      border-left: 10rpx solid $color;
      font-size: 34rpx;
      font-weight: 600;
    }
  }

  .hots_content {
    display: flex;
    flex-wrap: wrap;

    .hot_item {
      width: 50%;
      border: 10rpx solid #fff;

      img {}
    }
  }
}
</style>
