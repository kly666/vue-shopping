<template>
  <div ref="wrapper">
    <div class="content">
      <slot></slot>
    </div>
  </div>
</template>

<script>
import BetterScroll from "better-scroll";

export default {
  name: "Scroll",
  mounted() {
    let bs = new BetterScroll(this.$refs.wrapper, {
      pullUpLoad: true,
    });
    bs.on("scroll", (position) => {
      this.$emit("scrollPosition", position);
    });
    bs.on("pullingUp", () => {
      console.log(22);
      setTimeout(() => {
        bs.finishPullUp();
      }, 2000);
    });
  },
};
</script>

<style>
</style>