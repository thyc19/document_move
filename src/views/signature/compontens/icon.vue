<template>
  <div
    @touchmove.prevent
    class="lookDetail"
    ref="dragBox"
    @touchstart="touchstartHandle('dragBox',$event)"
    @touchmove="touchmoveHandle('dragBox',$event)"
  >
    <van-icon name="plus" color="#fff" size="20" />
  </div>
</template>

<script>
export default {
  data() {
    return {
      coordinate: {
        client: {},
        elePosition: {},
      },
    };
  },
  methods: {
    touchstartHandle(refName, e) {
      let element = e.targetTouches[0];
      // 记录点击的坐标
      this.coordinate.client = {
        x: element.clientX,
        y: element.clientY,
      };
      // 记录需要移动的元素坐标
      this.coordinate.elePosition.left = this.$refs[refName].offsetLeft;
      this.coordinate.elePosition.top = this.$refs[refName].offsetTop;
    },
    touchmoveHandle(refName, e) {
      let element = e.targetTouches[0];
      // 根据初始 client 位置计算移动距离(元素移动位置=元素初始位置+光标移动后的位置-光标点击时的初始位置)
      let x =
        this.coordinate.elePosition.left +
        (element.clientX - this.coordinate.client.x);
      let y =
        this.coordinate.elePosition.top +
        (element.clientY - this.coordinate.client.y);
      // 限制可移动距离，不超出可视区域
      x =
        x <= 0
          ? 0
          : x >= innerWidth - this.$refs[refName].offsetWidth
          ? innerWidth - this.$refs[refName].offsetWidth
          : x;
      y =
        y <= 0
          ? 0
          : y >= innerHeight - this.$refs[refName].offsetHeight
          ? innerHeight - this.$refs[refName].offsetHeight
          : y;
      // 移动当前元素
      this.$refs[refName].style.left = x + "px";
      this.$refs[refName].style.top = y + "px";
    },
  },
};
</script>

<style scoped lang="scss">
.lookDetail {
  position: fixed;
  right: 10px;
  bottom: 25%;
  z-index: 99;
  width: 44px;
  height: 44px;
  background-color: #0082ef;
  border-radius: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 2px solid white;
  padding: 2px;
  div {
    display: block;
    font-size: 12px;
    margin-left: 8px;
    color: #000;
  }
}
</style>