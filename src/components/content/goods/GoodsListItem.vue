<template>
  <div class="goods-item">
    <img :src="goodsItem.show.img" alt="" @load="imageLoad" />
    <div class="goods-info">
      <p>{{ goodsItem.title }}</p>
      <div class="infos">
        <span class="price">{{ goodsItem.price }}</span>
        <span class="collect">{{ goodsItem.cfav }}</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "GoodsListItem",
  props: {
    goodsItem: {
      type: Object,
      default() {
        return {};
      },
    },
  },
  methods: {
    imageLoad() {
      this.$bus.$emit('itemImageLoad')
    },
  },
};
</script>

<style scoped>
  .goods-item {
    width: 48%;

    margin-bottom: 10px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
  }
  .goods-item img {
    width: 100%;
    border-radius: 5px;
    margin-bottom: 5px;
  }
  .goods-info {
    padding: 0 5px;
    font-size: 14px;
  }
  .goods-info p {
    overflow: hidden;
    /* 弹性伸缩盒子模型显示 */
    display: -webkit-box;
    text-overflow: ellipsis;
    /* 设置或检索伸缩盒对象的子元素的排列方式 */
    -webkit-box-orient: vertical;
    /* 限制一个块元素显示的文本的行数 */
    -webkit-line-clamp: 1;
  }
  .infos {
    text-align: center;
  }
  .infos .price {
    color: var(--color-high-text);
    padding-right: 8px;
  }
  .infos .collect::before {
    content: "★";
    font-size: 22px;
    color: yellow;
    line-height: 0.8;
  }
</style>