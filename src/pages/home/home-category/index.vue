<template>
<view class="category_list">
  <view class="category_item" v-for="item in category" :key="item.id">
    <image :src="item.cover"></image>
    <view class="category name">{{ item.name }}</view>
  </view>
</view>
</template>

<script>
export default {
  data() {
    return {
      // 分类数组
      category: [],
    }
  },
  mounted() {
    this.getList()
  },
  methods: {
    // 获取页面信息
    getList() {
      this.request({
        url: 'http://157.122.54.189:9088/image/v1/vertical/category',
      }).then((res) => {
        this.category = res.res.category
        console.log(this.category)
      })
    },
  },
}
</script>

<style lang="scss"></style>
