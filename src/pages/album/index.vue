<template>
<view>
  <!--专辑背景-->
  <view class="album_bg">
    <image mode="aspectFill" :src="album.cover"></image>
    <view class="album_info">
      <view class="album_name">{{album.name}}</view>
      <view class="album_btn">关注专辑</view>
    </view>
  </view>

</view>
</template>

<script>
export default {
  data() {
    return {
      // 请求参数
      params: {
        limit: 30,
        order: 'new',
        skip: 0,
        // 下拉获取数据时，设置成0
        first: 1
      },
      // 接收页面参数id值
      id: -1,
      // 专辑信息
      album: {},
      // 专辑列表图
      wallpaper: []
    }
  },
  onLoad(options) {
    // this.id = options.id
    this.id = '5e22780fe7bce739db126f29'
    this.getList()
    console.log(options.id)
  },
  methods: {
    getList() {
      this.request({
          url: `http://157.122.54.189:9088/image/v1/wallpaper/album/${this.id}/wallpaper`,
          data: this.params
        })
        .then(result => {
          this.album = result.res.album
          this.wallpaper = result.res.wallpaper
          console.log(result)
        })
    }
  }
}
</script>

<style lang="scss" scoped>
.album_bg {
  position: relative;

  image {
    width: 100%;
  }

  .album_info {
    display: flex;
    justify-content: space-between;
    align-items: center;
    position: absolute;
    bottom: 0;
    left: 0;
    width: 100%;
    height: 80rpx;
    padding: 30rpx;
    font-size: 36rpx;
    color: #fff;

    .album_name {
      font-weight: 600;
    }

    .album_btn {
      padding: 10rpx 20rpx;
      font-size: 24rpx;
      border-radius: 10rpx;
      background-color: $color;
    }
  }
}
</style>
