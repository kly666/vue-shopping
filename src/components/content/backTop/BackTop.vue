<template>
  <div class="back-top">
    <img src="~assets/img/common/top.png" alt="" />
  </div>
</template>

<script>
export default {
  name: "BackTop",
};
</script>

<style lang="less">
.back-top {
  position: absolute;
  bottom: 10%;
  right: 0;
  z-index: 10;
  text-align: center;
  img {
    width: 50%;
  }
}
</style>