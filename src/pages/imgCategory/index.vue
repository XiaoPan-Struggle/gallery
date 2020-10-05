<template>
<view>
  <view>
    <view class="category_tab">
      <view class="category_tab_title">
        <view class="title_inner">
          <uni-segmented-control :current="current" :values="items.map((v) => v.title)" @clickItem="onClickItem" style-type="text" active-color="#d4237a"></uni-segmented-control>
        </view>
        <view class="iconfont iconsearch"></view>
      </view>
      <view class="content">
        123
      </view>
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
          title: '推荐',
        },
        {
          title: '分类',
        },
      ],
      current: 0,
    }
  },
  methods: {
    onClickItem(e) {
      if (this.current !== e.currentIndex) {
        this.current = e.currentIndex
        uni.setNavigationBarTitle({
          title: this.items[e.currentIndex].title,
        })
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
}
</style>
