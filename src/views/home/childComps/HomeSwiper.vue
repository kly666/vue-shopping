<template>
  <div id="home-swiper">
    <swiper>
      <swiper-item v-for="item in banners">
        <a :href="item.link"><img :src="item.image" alt="" /></a>
      </swiper-item>
    </swiper>
  </div>
</template>

<script>
import Swiper from "components/common/swiper/Swiper.vue";
import SwiperItem from "components/common/swiper/SwiperItem.vue";
export default {
  components: { Swiper, SwiperItem },
  props: {
    banners: {
      type: Array,
      default() {
        return [];
      },
    },
  },
};
</script>

<style>
</style>