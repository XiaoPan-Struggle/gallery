<template>
<scroll-view scroll-y="true" enable-flex @scrolltolower='handleScrolltolower' class="video_main">
  <view class="video_item" v-for="(item) in videowp" :key="item.id" @click="handleGovideo(item)">
    <image mode="widthFix" :src="item.img"></image>
  </view>
</scroll-view>
</template>

<script>
export default {
  props: {
    objUrl: {
      type: Object
    }
  },
  mounted() {
    this.getList()
  },
  watch: {
    objUrl() {
      this.videowp = []
      this.getList()
    }
  },
  data() {
    return {
      videowp: [],
      hasMore: true
    }
  },
  methods: {
    getList() {
      this.request({
        url: this.objUrl.url,
        data: this.objUrl.params
      }).then(result => {
        console.log(result)
        if (result.res.videowp.length === 0) {
          this.hasMore = false;
          uni.showToast({
            title: '没有更多数据了',
            icon: 'none'
          })
          return
        }
        this.videowp = [...this.videowp, ...result.res.videowp];
      })
    },
    // 分页事件
    handleScrolltolower() {
      if (this.hasMore) {
        this.objUrl.params.skip += this.objUrl.params.limit
        this.getList()
      }
    },
    // 点击事件
    handleGovideo(item) {
      // 将数据存入全局
      getApp().globalData.video = item
      // 跳转
      uni.navigateTo({
        url: '/pages/videoPlay/index'
      })
    }
  },
}
</script>

<style lang="scss">
.video_main {
  display: flex;
  flex-wrap: wrap;
  height: calc(100vh - 36px);

  .video_item {
    width: 33.33%;
    border: 5rpx solid #fff;
  }
}
</style>
