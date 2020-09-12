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
.album_swiper {
  swiper {
    height: calc(750rpx / 2.3);

    image {
      height: 100%;
    }
  }
}
</style>
