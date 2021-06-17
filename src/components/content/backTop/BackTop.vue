<template>
  <div class="back-top">
  </div>
</template>

<script>
export default {
  name: 'BackTop',
}
</script>

<style scoped>
  .back-top {
    z-index: 10;
    position: fixed;

    background: url("../../../assets/img/common/top.png") no-repeat center;
    background-size: 100%;

    width: 45px;
    height: 45px;
    border-radius: 50%;
    right: 2%;
    bottom: 50px;
  }
</style>