<template>
  <scroll class="content" :pull-up-load="true">
    <div class="category-list">
      <div
        class="list-item"
        v-for="(item, index) in categories"
        :key="index"
        @click="itemClick(item, index)"
        :class="{ active: currentIndex === index }"
      >
        {{ item.title }}
      </div>
    </div>
  </scroll>
</template>

<script>
import Scroll from "components/common/scroll/Scroll";
export default {
  name: "CategoryList",
  props: {
    categories: Array,
    default() {
      return [];
    },
  },
  data() {
    return {
      currentIndex: 0,
    };
  },
  components: {
    Scroll,
  },
  methods: {
    itemClick(item, index) {
      this.currentIndex = index;
      const obj = {
        maitKey: item.maitKey,
        index,
      };
      this.$emit("categoryClick", obj);
    },
  },
};
</script>

<style scoped>
.category-list {
  display: flex;
  flex-direction: column;
  background-color: #f6f6f6;
  width: 20%;
}
.content {
  position: absolute;
  top: 44px;
  bottom: 49px;
  left: 0;
  right: 0;
  overflow: hidden;
}
.list-item {
  text-align: center;
  height: 40px;
  line-height: 40px;
  font-size: 12px;
}
.active {
  color: var(--color-high-text);
  background-color: #fff;
  border-left: 2px solid var(--color-tint);
}
</style>