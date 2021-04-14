<template>
  <div id="home-recommend">
    <div v-for="item in recommends" class="home-recommend-item">
      <a :href="item.link">
        <img :src="item.image" alt="" />
        <p>{{ item.title }}</p>
      </a>
    </div>
  </div>
</template>

<script>
export default {
  name: "HomeRecommends",
  props: {
    recommends: {
      type: Array,
      default() {
        return [];
      },
    },
  },
};
</script>

<style lang="less">
#home-recommend {
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  width: 100%;
  padding-bottom: 5px;
  border-bottom: 10px solid #eee;
  .home-recommend-item {
    flex: 1;
    padding: 10px 5px;
    text-align: center;
    font-size: 14px;
    img {
      width: 100%;
    }
  }
}
</style>