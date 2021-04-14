<template>
  <div id="home">
    <nav-bar BackgroundColor="#ff8e96" class="nav-bar-text">
      <template #NavBarMid> <h5>首页</h5> </template>
    </nav-bar>
    <scroll class="wrapper" @scrollPosition="scrollPosition">
      <home-swiper :banners="banners"></home-swiper>
      <home-recommend :recommends="recommends"></home-recommend>
      <popular></popular>
      <tab-control
        :tabItems="['流行', '精选', '新款']"
        class="tab-control"
        @tabClick="tabClick"
      ></tab-control>
      <home-goods :goods="goods[currentType].list"></home-goods>
    </scroll>
    <back-top @click.native="backTopClick"></back-top>
  </div>
</template>

<script>
import NavBar from "components/common/navbar/NavBar";
import TabControl from "components/content/tabControl/TabControl.vue";
import Scroll from "components/common/scroll/Scroll.vue";

import HomeSwiper from "./childComps/HomeSwiper.vue";
import HomeRecommend from "./childComps/HomeRecommend.vue";
import Popular from "./childComps/Popular.vue";
import HomeGoods from "components/content/homeGoods/HomeGoods.vue";
import BackTop from "components/content/backTop/BackTop.vue";

import { getHomeMultidata, getHomeGoods } from "network/home.js";

export default {
  name: "Home",
  components: {
    NavBar,
    TabControl,

    HomeSwiper,
    HomeRecommend,
    Popular,
    HomeGoods,
    Scroll,
    BackTop,
  },
  data() {
    return {
      banners: null,
      dKeyword: null,
      keywords: null,
      recommends: null,
      goods: {
        pop: { page: 0, list: [] },
        sell: { page: 0, list: [] },
        new: { page: 0, list: [] },
      },

      currentType: "pop",
    };
  },
  methods: {
    /**
     * 网络请求数据
     */
    getHomeMultidata() {
      getHomeMultidata().then((res) => {
        this.banners = res.data.banner.list;
        this.dKeyword = res.data.dKeyword.list;
        this.keywords = res.data.keywords.list;
        this.recommends = res.data.recommend.list;
      });
    },
    getHomeGoods(type) {
      const page = this.goods[type].page + 1;
      getHomeGoods(type, page).then((res) => {
        this.goods[type].list.push(...res.data.list);
        this.goods[type].page = page;
      });
    },

    /**
     * 事件监听函数
     */
    tabClick(index) {
      switch (index) {
        case 0:
          this.currentType = "pop";
          break;
        case 1:
          this.currentType = "sell";
          break;
        case 2:
          this.currentType = "new";
          break;
      }
    },
    scrollPosition(position) {
      console.log(position);
    },
    backTopClick() {
      console.log(22);
      window.scroll(0, 0);
    },
  },
  created() {
    this.getHomeMultidata();
    this.getHomeGoods("pop");
    this.getHomeGoods("sell");
    this.getHomeGoods("new");
  },
};
</script>

<style>
#home {
  position: relative;
  height: 100vh;
}
.nav-bar-text {
  color: #fff;
  font-size: 24px;
}

.wrapper {
  height: calc(100% - 44px);
  overflow: hidden;
}
</style>