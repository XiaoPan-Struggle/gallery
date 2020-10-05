<template>
<view class="category_list">
  <view class="category_item" v-for="item in category" :key="item.id">
    <image mode="aspectFill" :src="item.cover"></image>
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

<style lang="scss">
.category_list {
  display: flex;
  flex-wrap: wrap;

  .category_item {
    width: 33.3%;
    position: relative;
    border: 5rpx solid #fff;

    image {
      height: 240rpx;
    }

    .category.name {
      position: absolute;
      left: 0;
      bottom: 0;
      width: 100%;
      height: 40rpx;
      font-size: 40rpx;
      font-weight: 500;
      color: #fff;
      // css3 渐变
      background-image: linear-gradient(to right top,
          rgba(0, 0, 0, 0.2),
          rgba(0, 0, 0, 0));
      display: flex;
      align-items: center;
      padding-left: 20rpx;
      padding-bottom: 10rpx;
    }
  }
}
</style>
