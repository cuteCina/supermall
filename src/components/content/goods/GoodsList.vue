<template>
  <div class="goods">
    <goods-list-item v-for="(item, i) in goods" :key="item.iid + i" :goods-item="item" class="goods-item"></goods-list-item>
  </div>
</template>

<script>
import GoodsListItem from './GoodsListItem'

export default {
  name: "GoodsList",
  props: {
    goods: {
      type: Array,
      default() {
        return [];
      },
    },
  },
  components: {
    GoodsListItem
  }
};
</script>

<style scoped>
.goods {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  background-color: #fff;
  padding: 0 1%;
}

</style>