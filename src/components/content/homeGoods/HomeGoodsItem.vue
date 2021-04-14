<template>
  <div class="home-goods-item">
    <a :href="item.link">
      <img :src="item.show.img" alt="" />
      <div>
        <p>{{ item.title }}</p>
      </div>
    </a>
  </div>
</template>

<script>
export default {
  name: "HomeGoodsItem",
  props: {
    item: {
      type: Object,
      default() {
        return {};
      },
    },
  },
};
</script>

<style lang="less">
.home-goods-item {
  padding-top: 10px;
  width: 48%;

  img {
    width: 100%;
    border-radius: 10px;
  }
  div {
    p {
      font-size: 14px;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }
  }
}
</style>