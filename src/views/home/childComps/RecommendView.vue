<template>
  <div class="recommend">
    <div v-for="item in recommends" :key="item.title" class="recommend-item">
      <a :href="item.link">
        <img :src="item.image" alt="">
        <span>{{item.title}}</span>
      </a>
    </div>
  </div>
</template>

<script>
export default {
  name: 'RecommendView',
  props: {
    recommends: {
      type: Array,
      default() {
        return []
      }
    }
  }
}
</script>

<style scoped>
  .recommend {
    display: flex;
    justify-content: space-around;
    padding: 10px 0 20px;
    border-bottom: 10px solid #eee;
    font-size: 14px;
  }
  .recommend-item {
    flex: 1;
    width: 25%;
    margin: 5px;
  }
  .recommend-item a {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .recommend-item a img {
    width: 100%;
    vertical-align: middle;
    margin-bottom: 10px;
  }
</style>>
