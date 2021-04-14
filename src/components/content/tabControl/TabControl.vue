<template>
  <div class="tab-control">
    <div
      class="tab-control-item"
      v-for="(item, index) in tabItems"
      @click="itemClick(index)"
      :class="{ active: index === currentIndex }"
    >
      <span>{{ item }}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: "TabControl",
  props: {
    tabItems: {
      type: Array,
      default() {
        return ["左", "中", "右"];
      },
    },
  },
  data() {
    return {
      currentIndex: 0,
    };
  },
  methods: {
    itemClick(index) {
      this.currentIndex = index;
      this.$emit("tabClick", index);
    },
  },
};
</script>

<style lang="less">
.tab-control {
  display: flex;
  height: 20px;
  line-height: 20px;
  font-size: 16px;
  text-align: center;
  background-color: #fff;
  z-index: 9;
  .tab-control-item {
    flex: 1;
    span {
      padding: 0 2px 2px;
      border-bottom: 2px solid var(--color-tint);
    }
  }
}
.active {
  color: var(--color-high-text);
}
</style>