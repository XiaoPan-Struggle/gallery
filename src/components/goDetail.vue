<template>
<view @click="handleClick">
  <slot></slot>
</view>
</template>

<script>
export default {
  props: {
    list: Array,
    index: Number
  },
  methods: {
    handleClick() {
      // 数据缓存
      getApp().globalData.imgList = this.list
      getApp().globalData.imgIndex = this.index
      // 跳转路由
      uni.navigateTo({
        url: '/pages/imgDetail/index'
      })
    }
  }
}
</script>

<style lang="scss">
</style>
