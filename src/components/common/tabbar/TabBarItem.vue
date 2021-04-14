// 使用 TabBarItem 组件时可以传入三个插槽内容，其次可以传入 link="路由路径" route.path 路径，还能传入该 tabbaritem icon下面文字的颜色，使用  ActiveColor="字体颜色" 

<template>
  <div class="tab-bar-item" @click="ItemClick">
    <div v-if="!isActive"><slot name="item-icon"></slot></div>
    <div v-else><slot name="item-icon-active"></slot></div>
    <div :style="ActiveColorStyle"><slot name="item-text"></slot></div>
  </div>
</template>

<script>
export default {
  name: "TabBarItem",
  data() {
    return {
      // isActive: true,
    };
  },
  computed: {
    isActive() {
      return this.$route.path.indexOf(this.link) !== -1;
    },
    ActiveColorStyle() {
      return this.isActive ? { color: this.ActiveColor } : {};
    },
  },
  //接收父组件传递过来的 link
  props: {
    link: String,
    ActiveColor: {
      type: String,
      default: "#ff5777",
    },
  },
  methods: {
    ItemClick() {
      this.$router.push("/location").catch((err) => {
        console.log(err);
      }); // 只需添加这么一句代码，报错问题就能解决了
      this.$router.replace(this.link);
    },
  },
};
</script>

<style lang="less">
.tab-bar-item {
  flex: 1;
  text-align: center;
  font-size: 14px;

  img {
    margin-top: 3px;
    width: 24px;
    height: 24px;
    vertical-align: middle;
    margin-bottom: 2px;
  }
}
</style>