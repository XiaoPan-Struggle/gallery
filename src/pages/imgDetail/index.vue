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
    <swiper-action @swiperAction='handleSwiperAction'>
      <image mode="widthFix" :src="imgDetail.thumb"></image>
    </swiper-action>
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

  <!-- 最热评论 -->
  <view class="comment hot">
    <view class="comment title">
      <text class="iconfont iconhot1"></text>
      <text>最热评论</text>
    </view>
    <view class="comment_list">
      <view class="comment_item" v-for="item in hot" :key="item.id">
        <view class="comment_cover">
          <image :src="item.user.avatar"></image>
        </view>

        <view class="comment_info">
          <view class="comment_name">{{item.user.name}}</view>
          <view class="comment_time">{{item.atime}}</view>
          <view class="comment_info_content">
            <text>{{item.content}}</text>
            <text class="iconfont icondianzan">{{item.user.follower}}</text>
          </view>
        </view>
      </view>
    </view>
  </view>

  <!-- 最新评论 -->
  <view class="comment new">
    <view class="comment title">
      <text class="iconfont iconpinglun"></text>
      <text>最新评论</text>
    </view>
    <view class="comment_list">
      <view class="comment_item" v-for="item in comment" :key="item.id">
        <view class="comment_cover">
          <image :src="item.user.avatar"></image>
        </view>

        <view class="comment_info">
          <view class="comment_name">{{item.user.name}}</view>
          <view class="comment_time">{{item.atime}}</view>
          <view class="comment_info_content">
            <text>{{item.content}}</text>
            <text class="iconfont icondianzan">{{item.user.follower}}</text>
          </view>
        </view>
      </view>
    </view>
  </view>

</view>
</template>

<script>
import moment from 'moment'
import swiperAction from '@/components/swiperAction'
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
      hot: [],
      // 当前渲染图片的 index
      imgIndex: 0,
      // 图片全对象
      imgList: [],
      // 假数据
      user: {
        name: 'Kiko',
        avatar: 'http://img0.adesk.com/download/5dee04f804220801b33bebf7'
      }
    }
  },
  components: {
    swiperAction
  },
  onLoad() {
    const {
      imgIndex,
      imgList
    } = getApp().globalData;
    this.imgIndex = imgIndex
    // 页面赋值
    this.imgDetail = imgList[this.imgIndex]
    // 当该数据没有user时，就用假数据
    if (!this.imgDetail.user) {
      this.imgDetail.user = this.user
    }
    // 时间数据格式处理
    this.imgDetail.cnTime = moment(this.imgDetail.atime * 1000).fromNow()
    // 获取评论数据
    this.getComments(this.imgDetail.id)

  },
  methods: {
    getData() {
      const {
        imgList
      } = getApp().globalData;
      // 页面赋值
      this.imgDetail = imgList[this.imgIndex]
      // 时间数据格式处理
      this.imgDetail.cnTime = moment(this.imgDetail.atime * 1000).fromNow()
      // 获取评论数据
      this.getComments(this.imgDetail.id)
    },
    getComments(id) {
      this.request({
          url: `http://157.122.54.189:9088/image/v2/wallpaper/wallpaper/${id}/comment`
        })
        .then(result => {
          this.album = result.res.album;
          this.comment = result.res.comment;
          this.hot = result.res.hot;
          console.log(result.res)
        })
    },
    handleSwiperAction(e) {
      const {
        imgList
      } = getApp().globalData;
      console.log(imgList)
      if (e.direction === 'left' && this.imgIndex < imgList.length - 1) {
        console.log(123456)

        this.imgIndex++;
        this.getData();
      } else if (e.direction === 'right' && this.imgIndex > 0) {
        this.imgIndex--;
        this.getData();
      } else [
        uni.showToast({
          title: '没有数据了',
          icon: 'none'
        })
      ]
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
        padding-left: 10rpx;
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

// 热门评论
.comment {
  .comment.title {
    padding: 20rpx;
    border-top: 15rpx solid #f1f1f1;
    color: #333;
    font-weight: 600;

    text.iconfont {
      margin-right: 20rpx;
      font-size: 44rpx;
      color: $color;
    }
  }

  .comment_list {
    .comment_item {
      display: flex;
      border-bottom: 15rpx solid #f1f1f1;

      .comment_cover {
        padding: 10rpx;

        image {
          width: 80rpx;
          height: 80rpx;
        }
      }

      .comment_info {
        width: 80%;
        padding: 10rpx 10rpx;

        .comment_name {
          color: #ccc;
        }

        .comment_time {
          padding: 30rpx 0;
          padding-top: 10rpx;
          color: #868b8f;
        }

        .comment_info_content {
          position: relative;
          width: 100%;
          font-size: 30rpx;
          font-weight: 600;

          text {
            color: #000;
          }

          text.iconfont.icondianzan {
            position: absolute;
            top: 0;
            right: 10rpx;
          }
        }
      }
    }
  }
}
</style>
