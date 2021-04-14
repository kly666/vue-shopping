<template>
  <div class="nav-bar" :style="{ backgroundColor: BackgroundColor }">
    <div class="nav-bar-left">
      <slot name="NavBarLeft"></slot>
    </div>
    <div class="nav-bar-mid">
      <slot name="NavBarMid"></slot>
    </div>
    <div class="nav-bar-right">
      <slot name="NavBarRight"></slot>
    </div>
  </div>
</template>

<script>
export default {
  name: "NavBar",
  props: {
    BackgroundColor: {
      //为 navbar 添加自定义背景颜色
      type: String,
      default: "#ff8e96",
    },
  },
};
</script>

<style lang="less">
.nav-bar {
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  height: 44px;
  line-height: 44px; // 顶部导航栏一般为 44px ，若有状态栏则为 64px ，状态栏占据 20px;此外直接使用 line-height 即可，这个是有内容时会撑起来的
  text-align: center;
  background-color: #ff8e96;
  .nav-bar-left,
  .nav-bar-right {
    width: 60px;
  }
  .nav-bar-mid {
    flex: 1;
  }
}
</style>