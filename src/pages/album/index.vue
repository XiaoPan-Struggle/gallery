<template>
<view>
  专辑详情
</view>
</template>

<script>
export default {
  data() {
    return {
      id: -1
    }
  },
  onLoad(options) {
    this.id = options.id
    console.log(options.id)
  }
}
</script>

<style lang="scss">
</style>
