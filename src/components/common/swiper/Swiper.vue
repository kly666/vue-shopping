<template>
  <div
    id="hy-swiper"
    @touchstart="touchStart"
    @touchmove="touchMove"
    @touchend="touchEnd"
  >
    <div class="swiper">
      <!-- <div v-for="item in banners" class="swiper-item">
        <a :href="item.link"><img :src="item.image" alt="" /></a>
      </div> -->
      <!-- <swiper-item v-for="item in banners">
        <a :href="item.link"><img :src="item.image" alt="" /></a>
      </swiper-item> -->
      <slot></slot>
    </div>
    <div class="swiper-circle">
      <ul>
        <li
          v-for="(item, index) in swiperItemCount"
          :class="currentIndex == item ? 'swiperItemActive' : ''"
          @click="clickItem(item)"
        ></li>
      </ul>
    </div>
  </div>
</template>

<script>
import SwiperItem from "./SwiperItem";
export default {
  name: "Swiper",
  components: {
    SwiperItem,
  },
  props: {
    banners: {
      // 图片内容
      type: Array,
      default() {
        return [];
      },
    },
  },
  data() {
    return {
      swiperItemCount: 0, // 轮播图数
      swiperStyle: "", // swiper的样式
      currentIndex: 1, // 当前的 index
      moveRatio: 0.25, // 跳转下一张图片所需的移动距离比例
    };
  },
  methods: {
    // 设置滚动位置
    setTransform(position) {
      this.swiperStyle.transform = `translate3d(${position}px, 0, 0)`;
      this.swiperStyle[
        "-webkit-transform"
      ] = `translate3d(${position}px), 0, 0`;
      this.swiperStyle["-ms-transform"] = `translate3d(${position}px), 0, 0`;
    },
    // 设置滚动过渡动画
    setTransition() {
      this.swiperStyle.transition = `transform 300ms`;
    },

    // 1.先操作 dom 元素
    handleDom() {
      // 获取 dom 对象
      let swiper = document.querySelector(".swiper");
      this.swiperItemCount = swiper.children.length;
      // 复制第一张图片和最后一张图片，分别放在最后面和最前面
      let nodeFirst = swiper.children[0].cloneNode(true);
      let nodeLast = swiper.children[this.swiperItemCount - 1].cloneNode(true);
      swiper.appendChild(nodeFirst); // 如果 cloneNode 报错，那么只能在 dom 中直接添加最后一个图片了
      swiper.insertBefore(nodeLast, swiper.children[0]);
      this.swiperStyle = swiper.style;
      this.totalWidth = swiper.offsetWidth;

      // 此时，currentIndex 的初始值为 1 ，那么先显示第二张图片
      this.setTransform(-this.currentIndex * this.totalWidth);
    },

    // 2. 实现用户触摸移动，图片也跟随移动，并根据滑动的方向跳转下一张图片的功能
    //触摸开始
    touchStart: function (e) {
      // 停止定时器
      this.stopTimer();

      // 获取并保存开始触摸的位置
      this.startX = e.touches[0].pageX;
    },
    // 触摸过程
    touchMove(e) {
      // 计算出用户拖动的距离
      this.currentX = e.touches[0].pageX;
      this.distance = this.currentX - this.startX;
      let currentPosition = -this.currentIndex * this.totalWidth;
      let moveDistance = this.distance + currentPosition;

      // 设置当前的位置，也就是随着触摸移动，图片也跟随移动
      this.setTransform(moveDistance);
    },
    // 触摸结束，根据滑动的方向跳转下一张图片
    touchEnd: function (e) {
      let distance_abs = Math.abs(this.distance);

      if (this.distance == 0) {
        return;
      } else if (
        this.distance < 0 &&
        distance_abs >= this.moveRatio * this.totalWidth
      ) {
        this.currentIndex++;
      } else if (
        this.distance > 0 &&
        distance_abs >= this.moveRatio * this.totalWidth
      ) {
        this.currentIndex--;
      }

      this.setTransition();
      this.setTransform(-this.currentIndex * this.totalWidth);
      this.currentScroll();

      this.swiperAuto();
    },

    // 3.判读第一张和最后一张图片的逻辑滚动，设置正确的滚动，抽取为函数
    currentScroll() {
      if (this.currentIndex == this.swiperItemCount + 1) {
        this.currentIndex = 1;
      } else if (this.currentIndex == 0) {
        this.currentIndex = this.swiperItemCount;
      }
      setTimeout(() => {
        this.swiperStyle.transition = "0ms";
        this.setTransform(-this.currentIndex * this.totalWidth);
      }, 300);
    },

    // 4. 实现点击小圆圈跳转相应的图片
    clickItem(item) {
      this.stopTimer();
      this.currentIndex = item;
      this.setTransform(-this.currentIndex * this.totalWidth);

      this.swiperAuto();
    },

    // 5.设置定时器，实现轮播功能
    swiperAuto() {
      this.playTimer = setInterval(() => {
        this.currentIndex++;
        this.setTransition();
        this.setTransform(-this.currentIndex * this.totalWidth);
        this.currentScroll();
      }, 2000);
    },
    stopTimer() {
      clearInterval(this.playTimer);
    },
  },
  mounted() {
    // 由于网络请求的图片存在时间延迟，所以等 0.5s 让图片渲染出来
    setTimeout(() => {
      this.handleDom();
      this.swiperAuto();
    }, 500);
  },
};
</script>

<style lang="less">
#hy-swiper {
  position: relative;
  overflow: hidden;
  .swiper {
    display: flex;
    flex-direction: row;
    flex-wrap: nowrap;
    width: 100%;
  }
  .swiper-circle {
    position: absolute;
    bottom: 10px;
    left: 50%;
    transform: translate(-50%, 0);
    ul li {
      list-style: none;
      float: left;
      width: 12px;
      height: 12px;
      border-radius: 50%;
      margin: 0 5px;
      background-color: #fff;
    }
    .swiperItemActive {
      background-color: var(--color-tint);
    }
  }
}
</style>