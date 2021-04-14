<template>
  <div class="home-goods">
    <home-goods-item v-for="item in goods" :key="item.iid" :item="item">
    </home-goods-item>
  </div>
</template>

<script>
import HomeGoodsItem from "./HomeGoodsItem.vue";

export default {
  components: { HomeGoodsItem },
  name: "HomeGoods",
  props: {
    goods: {
      type: Array,
      default() {
        return [];
      },
    },
  },
};
</script>

<style lange="less">
.home-goods {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: space-around;
  width: 100%;
  padding: 5px;
}
</style>