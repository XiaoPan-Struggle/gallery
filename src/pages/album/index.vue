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

  <!--专辑作者-->
  <view class="album_author">
    <view class="album_author_info">
      <image mode="widthFix" :src="album.user.avatar"></image>
      <view class="author_name">{{album.user.name}}</view>
    </view>
    <view class="album_author_desc">
      <text>{{album.desc}}</text>
    </view>
  </view>

  <!--专辑图列表-->
  <view class="album_list">
    <view class="album_item" v-for="(item,index) in wallpaper" :key="item.id">
      <go-detail :list='wallpaper' :index='index'>
        <image mode="aspectFill" :src="item.thumb+item.rule.replace('$<Height>',360)"></image>
      </go-detail>
    </view>
  </view>
</view>
</template>

<script>
import goDetail from '@/components/goDetail'
export default {
  components: {
    goDetail
  },
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
      wallpaper: [],
      // 是否还有下一页
      hasMore: true
    }
  },
  onLoad(options) {
    this.id = options.id
    this.getList()
  },
  // 页面触底事件
  onReachBottom() {
    if (this.hasMore) {
      // 修改参数
      this.params.skip += this.params.limit
      // 发送请求
      this.getList()
    }
  },
  methods: {
    getList() {
      this.request({
          url: `http://157.122.54.189:9088/image/v1/wallpaper/album/${this.id}/wallpaper`,
          data: this.params
        })
        .then(result => {
          if (Object.keys(this.album).length === 0) {
            this.album = result.res.album
          }
          // 没有下一页
          if (result.res.wallpaper.length === 0) {
            this.hasMore = false,
              uni.showToast({
                title: '极限啦！',
                icon: 'none'
              })
            return
          }
          this.wallpaper = [...this.wallpaper, ...result.res.wallpaper]
        })
    }
  }
}
</script>

<style lang="scss" scoped>
// 专辑背景
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
      font-weight: 600;
    }
  }
}

// 专辑作者
.album_author {
  padding: 20rpx;

  .album_author_info {
    display: flex;
    padding: 10rpx 0;

    image {
      width: 50rpx;
    }

    .author_name {
      padding-left: 20rpx;
      font-size: 30rpx;
      font-weight: 600;
      color: #333;
    }
  }

  .album_author_desc {
    padding-bottom: 30rpx;
  }
}

// 专辑图片
.album_list {
  display: flex;
  flex-wrap: wrap;

  .album_item {
    width: 33.3%;
    border: 3rpx solid #fff;

    image {
      height: 160rpx;
    }
  }
}
</style>
