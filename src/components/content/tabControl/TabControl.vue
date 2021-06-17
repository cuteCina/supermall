<template>
  <div class="tab-control">
    <div
      v-for="(item, i) in titles"
      :key="item"
      class="tab-control-item"
      :class="{ active: i === currentIndex }"
      @click="itemClick(i)"
    >
      <span>{{ item }}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: "TabControl",
  props: {
    titles: {
      type: Array,
      default() {
        return [];
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
      this.$emit('tabClick', index )
    },
  },
};
</script>

<style scoped>
  .tab-control {
    display: flex;
    text-align: center;
    height: 40px;
    line-height: 40px;
    background-color: #fff;
    padding-bottom: 5px;
  }
  .tab-control-item {
    flex: 1;
  }
  .active {
    color: var(--color-high-text);
    position: relative;
  }
  .active::after {
    content: "";
    /* display: block; */
    position: absolute;
    width: 40%;
    height: 3px;
    background-color: var(--color-tint);
    bottom: 0;
    left: 50%;
    transform: translateX(-50%);
  }
</style>>
