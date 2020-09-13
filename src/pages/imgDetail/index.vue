<template>
<view>

  <!-- 作者 -->
  <view class="user_info">
    <view class="user_icon">
      <image :src="imgDetail.user.avatar" mode="widthFix"></image>
    </view>
    <view class="user_desc">
      <view class="user_name">{{imgDetail.user.name}}</view>
      <view class="user_time">{{imgDetail.cnTime}}</view>
    </view>
  </view>

  <!-- 发布的大图 -->
  <view class="high_img">
    <image mode="widthFix" :src="imgDetail.newThumb"></image>
  </view>

  <!-- 点赞，收藏 -->
  <view class="user_rank">
    <view class="rank">
      <text class="iconfont icondianzan"> {{imgDetail.rank}} </text>
    </view>
    <view class="user_collect">
      <text class="iconfont iconshoucang"> 收藏 </text>
    </view>
  </view>

  <!-- 图片专辑 -->
  <view class="album_warp">
    <view class="album_title">相关</view>
    <view class="album_list">
      <view class="album_item" v-for="item in album" :key="item.id">
        <view class="album_cover">
          <image :src="item.cover"></image>
        </view>

        <view class="album_info">
          <view class="album_info_text">专辑</view>
          <view class="album_name">{{item.name}}</view>
          <text class="iconfont iconiconfontjiantou4"></text>
        </view>
      </view>
    </view>
  </view>

</view>
</template>

<script>
import moment from 'moment'
// 设置时间格式语言为中文
moment.locale("zh-cn")
export default {
  data() {
    return {
      imgDetail: {},
      // 专辑数据
      album: [],
      // 最新评论
      comment: [],
      // 热门评论
      hot: []
    }
  },
  onLoad() {
    const {
      imgIndex,
      imgList
    } = getApp().globalData;
    this.imgDetail = imgList[imgIndex]
    // 直接在路径上写入会报错，因为还没有拿到，路径会是undefined
    this.imgDetail.newThumb = this.imgDetail.thumb + this.imgDetail.rule.replace('$<Height>', 360)
    // 时间数据格式处理
    this.imgDetail.cnTime = moment(this.imgDetail.atime * 1000).fromNow()
    // 获取评论数据
    console.log(this.imgDetail.id)
    this.getComments(this.imgDetail.id)
  },
  methods: {
    getComments(id) {
      this.request({
          url: `http://157.122.54.189:9088/image/v2/wallpaper/wallpaper/${id}/comment`
        })
        .then(result => {
          this.album = result.res.album;
          this.comment = result.res.comment;
          this.hot = result.res.hot;
          console.log(result.res.album)
        })
    }
  }
}
</script>

<style lang="scss" scoped>
// 作者
.user_info {
  display: flex;
  padding: 30rpx;

  .user_icon {
    border-radius: 44rpx;
    overflow: hidden;

    image {
      width: 88rpx;
    }
  }

  .user_desc {
    display: flex;
    flex-direction: column;
    justify-content: center;
    padding-left: 30rpx;
    font-size: 26rpx;

    .user_name {
      font-weight: 600;
    }

    .user_time {
      color: #d3d3d3;
    }
  }
}

// 点赞
.user_rank {
  display: flex;
  text-align: center;
  border-bottom: 5rpx solid #eeeeee;

  .rank {
    flex: 1;
    padding: 20rpx 0;
  }

  .user_collect {
    flex: 1;
    padding: 20rpx 0;
  }
}

// 图片专辑
.album_warp {
  .album_title {
    padding: 20rpx 20rpx;
    font-size: 30rpx;
    font-weight: 600;
  }

  .album_list {
    .album_item {
      display: flex;
      padding: 10rpx 0;

      .album_cover {
        flex: 1;

        image {
          width: 180rpx;
          height: 180rpx;
        }
      }

      .album_info {
        position: relative;
        flex: 3;
        padding-left: 20rpx;

        .album_info_text {
          width: 100rpx;
          padding: 5rpx;
          border-radius: 10rpx;
          color: #fff;
          background-color: $color;
          text-align: center;
        }

        .album_name {
          padding-left: 20rpx;
        }

        .iconfont {
          position: absolute;
          top: 50%;
          right: 60rpx;
          font-size: 30rpx;
          transform: translateY(-50%);
        }
      }
    }
  }
}
</style>
