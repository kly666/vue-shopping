<template>
  <div class="swiper-item">
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: "SwiperItem",
};
</script>

<style lang="less">
.swiper-item {
  float: left;
  width: 100%;
  flex-shrink: 0; //收缩因子为0 则可以使得所有图片在同一行显示
  img {
    width: 100%;
  }
}
</style>