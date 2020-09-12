<template>
<scroll-view class="album_view" @scrolltolower="albumToLower" scroll-y>
  <!-- 轮播图 
    1.自动轮播 autoplay
    2.指示器 indicator-dots
    3.衔接轮播 circular

    swiper默认高度 150px
    image 默认高度 320px => 基本样式中重置了 100%
    默认高度 240px
  -->
  <view class="album_swiper">
    <swiper autoplay circular indicator-dots>
      <swiper-item v-for="item in banner" :key="item.id">
        <image mode="widthFix" :src="item.thumb"></image>
      </swiper-item>
    </swiper>
  </view>

  <!-- 专辑列表 -->
  <view class="album_list">
    <navigator class="album_item" v-for="item in album" :key="item.id" :url="`/pages/album/index?id=${item.id}`">
      <view class="album_img">
        <image mode="aspectFill" :src="item.cover"></image>
      </view>
      <view class="album_info">
        <view class="album_name">{{item.name}}</view>
        <view class="album_desc">{{item.desc}}</view>
        <view class="album_btn">
          <view class="album_attention">+ 关注</view>
        </view>
      </view>
    </navigator>
  </view>
</scroll-view>
</template>

<script>
export default {
  data() {
    return {
      // 传递参数
      params: {
        limit: 30,
        order: 'new',
        skip: 0
      },
      // 轮播图数据
      banner: [],
      // 专辑列表数据
      album: [],
      // 是否还有下一页
      hasMore: true
    }
  },
  mounted() {
    // 获取数据
    this.getList()
  },
  methods: {
    // 获取页面数据
    getList() {
      this.request({
          url: 'http://157.122.54.189:9088/image/v1/wallpaper/album',
          data: this.params
        })
        .then(result => {
          // 轮播图数据只需要接收一次
          if (this.banner.length === 0) {
            this.banner = result.res.banner
          }
          // 没有下一页
          if (result.res.album.length === 0) {
            this.hasMore = false,
              uni.showToast({
                title: '极限啦！',
                icon: 'none'
              })
            return
          }
          // 数据拼接 es6
          this.album = [...this.album, ...result.res.album]
        })
    },
    // 触底事件
    albumToLower() {
      if (this.hasMore) {
        // 修改参数
        this.params.skip += this.params.limit
        // 发送请求
        this.getList()
      }
    }
  }
}
</script>

<style lang="scss">
.album_view {
  // 视口-头部tab高
  height: calc(100vh - 36px);
}

// 轮播图
.album_swiper {
  swiper {
    height: calc(750rpx / 2.3);

    image {
      height: 100%;
    }
  }
}

// 专辑列表
.album_list {
  padding: 10rpx;

  .album_item {
    display: flex;
    padding: 10rpx 0;
    border-bottom: 1rpx solid #ccc;

    .album_img {
      flex: 1;
      padding: 10rpx;

      image {
        width: 200rpx;
        height: 200rpx;
      }
    }

    .album_info {
      flex: 2;
      padding: 0 10rpx;
      overflow: hidden;

      .album_name {
        padding: 10rpx 0;
        color: #000;
        font-size: 30rpx;
        font-weight: 600;
      }

      .album_desc {
        padding: 10rpx 0 30rpx 0;
        font-size: 24rpx;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
      }

      .album_btn {
        display: flex;
        flex-direction: row-reverse;

        .album_attention {
          display: inline-block;
          padding: 10rpx;
          border: 3rpx solid $color;
          color: $color;
        }
      }
    }
  }
}
</style>
