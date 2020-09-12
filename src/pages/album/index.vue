<template>
<view>
  专辑详情
</view>
</template>

<script>
export default {
  data() {
    return {
      // 请求参数
      params: {
        limit: 30,
        order: 'new',
        skip: 0,
        first: 1
      },
      // 接收页面参数id值
      id: -1,
      // 专辑信息
      album: {},
      // 专辑列表图
      wallpaper: []
    }
  },
  onLoad(options) {
    this.id = options.id
    this.getList()
    console.log(options.id)
  },
  methods: {
    getList() {
      this.request({
          url: `http://157.122.54.189:9088/image/v1/wallpaper/album/${this.id}/wallpaper`,
          data: this.params
        })
        .then(result => {
          this.album = result.res.album
          this.wallpaper = result.res.wallpaper
          console.log(result)
        })
    }
  }
}
</script>

<style lang="scss">
</style>
