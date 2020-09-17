<template>
<view @touchstart="handleTouchStart" @touchend="handleTouchEnd">
  <slot></slot>
</view>
</template>

<script>
export default {
  // 手指按下屏幕事件 touchstart
  // 手指离开屏幕事件 touchend
  // event.changedTouches[0].clientX 和 clientY   获取坐标
  // 1.记录用户按下屏幕的时间 Date.now()  注意：返回的时时间戳，1970 -1-1 到现在的毫秒数
  // 2.记录用户按下屏幕的坐标 x 和 y
  // 3.记录用户离开屏幕的时间 Date.now()
  // 4.记录用户离开屏幕的坐标 x 和 y
  // 5.根据两个时间 运算 判断 用户按下屏幕时长是否合法
  // 6.根据两对坐标 判断距离是否合法 判断 滑动方向
  //     判断用户滑动距离是否合法 => 取绝对值判断距离
  //     判断滑动的方向 =>  相减 正为右滑  负为左滑

  data() {
    return {
      // 开始时间
      startTime: 0,
      // 开始坐标
      startX: 0,
      startY: 0,
    }
  },
  methods: {
    handleTouchStart(event) {
      // 获取按下的当前时间
      this.startTime = Date.now()
      // 获取按下的当前坐标
      this.startX = event.changedTouches[0].clientX
    },
    handleTouchEnd(event) {
      // 获取离开的当前时间
      const endTime = Date.now()
      // 获取离开的当前坐标
      const endX = event.changedTouches[0].clientX
      // 判断按下的时长
      if (endTime - this.startTime > 2000) {
        // 时间太长就不执行
        return
      }
      // 声明一个变量 存储 方向
      let direction = ''
      // 结束坐标 - 开始坐标  Math.abs(x) 返回x 的绝对值
      if (Math.abs(endX - this.startX) > 10) {
        // 返回方向名词
        direction = endX - this.startX > 0 ? 'right' : 'left'
      } else {
        // 轻微滑动 无效
        return
      }
      this.$emit('swiperAction', {
        direction
      })
    },
  },
}
</script>
