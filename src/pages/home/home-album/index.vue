<template>
<view>
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
    <view class="album_item" v-for="item in album" :key="item.id">
      <view class="album_img">
        <image :src="item.cover"></image>
      </view>
      <view class="album_info">
        <view class="album_name">{{item.name}}</view>
        <view class="album_desc">{{item.desc}}</view>
        <view class="album_btn">
          <view class="album_attention">+ 关注</view>
        </view>
      </view>
    </view>
  </view>
</view>
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
      album: []
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
          console.log(result)
          this.banner = result.res.banner
          this.album = result.res.album
          console.log(this.banner, this.album)
        })
    }
  }
}
</script>

<style lang="scss">
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
