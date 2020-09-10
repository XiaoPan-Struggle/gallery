<template>
<view>

  <view>
    <uni-segmented-control :current="current" :values="items.map(v => v.title)" @clickItem="onClickItem" style-type="text" active-color="#d4237a"></uni-segmented-control>
    <view class="content">
      <view v-if="current === 0">
        <homereCommend></homereCommend>
      </view>
      <view v-if="current === 1">
        <homeCategory></homeCategory>
      </view>
      <view v-if="current === 2">
        <homeNew></homeNew>
      </view>
      <view v-if="current === 3">
        <homeAlbum></homeAlbum>
      </view>
    </view>
  </view>
</view>
</template>

<script>
import homeAlbum from './home-album';
import homeCategory from './home-category';
import homeNew from './home-new';
import homereCommend from './home-recommend';
import {
  uniSegmentedControl
} from '@dcloudio/uni-ui';
export default {
  components: {
    homeAlbum,
    homeCategory,
    homeNew,
    homereCommend,
    uniSegmentedControl
  },
  data() {
    return {
      items: [{
          title: '推荐'
        },
        {
          title: '分类'
        },
        {
          title: '最新'
        },
        {
          title: '专辑'
        }
      ],
      current: 0
    }
  },
  methods: {
    onClickItem(e) {
      if (this.current !== e.currentIndex) {
        this.current = e.currentIndex;
      }
    }
  }
}
</script>

<style lang="scss"></style>
