<template>
<view>
  <view class="category_tab">
    <view class="category_tab_title">
      <view class="title_inner">
        <uni-segmented-control :current="current" :values="items.map((v) => v.title)" @clickItem="onClickItem" style-type="text" active-color="#d4237a"></uni-segmented-control>
      </view>
      <view class="iconfont iconsearch"></view>
    </view>
    <scroll-view enable-flex scroll-y @scrolltolower="handleScrolltolower" class="category_tabs_content" v-if="isshow">
      <view class="category_item" v-for="item in vertical" :key="item.id">
        <image :src="item.thumb" mode="widthFix"></image>
      </view>
    </scroll-view>
    <view class="category_not_data" v-else>
      Not Data
    </view>
  </view>
</view>
</template>

<script>
import {
  uniSegmentedControl
} from '@dcloudio/uni-ui'
export default {
  components: {
    uniSegmentedControl,
  },
  data() {
    return {
      items: [{
          title: '最新',
          order: 'new',
        },
        {
          title: '热门',
          order: 'hot',
        },
      ],
      current: 0,
      params: {
        limit: 30,
        skip: 0,
        order: 'new',
      },
      id: 0,
      // 页面初始化数据
      vertical: [],
      isshow: true,
      // 有无下一页
      hasMore: true,
    }
  },
  onLoad(options) {
    this.id = options.id
    this.getList()
  },
  methods: {
    // 点击标题
    onClickItem(e) {
      if (this.current !== e.currentIndex) {
        this.current = e.currentIndex
      } else {
        return
      }
      this.params.order = this.items[e.currentIndex].order
      this.params.skip = 0
      this.vertical = []
      this.getList()
    },
    // 发送请求
    getList() {
      this.request({
        url: `http://157.122.54.189:9088/image/v1/vertical/category/${this.id}/vertical`,
        data: this.params,
      }).then((res) => {
        console.log(res.res.vertical)
        if (this.vertical.length === 0) {
          this.isshow = false
        }
        if (res.res.vertical.length === 0) {
          this.hasMore = false
          uni.showToast({
            title: '极限啦!',
            icon: 'none',
          })
          return
        }
        this.vertical = [...this.vertical, ...res.res.vertical]
        this.isshow = true
        this.hasMore = true
      })
    },
    // 滚动分页
    handleScrolltolower() {
      if (this.hasMore) {
        this.params.skip += this.params.limit
        this.getList()
      }
    },
  },
}
</script>

<style lang="scss">
.category_tab {
  .category_tab_title {
    position: relative;

    .title_inner {
      width: 60%;
      margin: 0 auto;
    }

    .iconsearch {
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
      right: 5%;
    }
  }

  .category_tabs_content {
    display: flex;
    flex-wrap: wrap;
    padding-top: 5rpx;
    height: calc(100vh - 72rpx);

    .category_item {
      width: 50%;
      border: 5rpx solid #fff;

      image {}
    }
  }

  .category_not_data {
    height: 100vh;
    padding-bottom: 30rpx;
    display: flex;
    align-items: center;
    justify-content: center;
    font-weight: 600;
    font-size: 60rpx;
  }
}
</style>
