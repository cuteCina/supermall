<template>
  <div id="home">
    <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>

    <tab-control
      :titles="['流行', '新款', '精选']"
      @tabClick="tabClick"
      ref="tabControl1"
      v-show="isTabFixed"
      class="tab-control"
    ></tab-control>
    <scroll
      class="content"
      ref="scroll"
      :probe-type="3"
      @scroll="contentScroll"
      :pull-up-load="true"
    >
      <home-swiper
        :banners="banners"
        @swiperImageLoad="swiperImageLoad"
      ></home-swiper>
      <recommend-view :recommends="recommends"></recommend-view>
      <feature-view></feature-view>
      <tab-control
        :titles="['流行', '新款', '精选']"
        @tabClick="tabClick"
        ref="tabControl2"
      ></tab-control>
      <goods-list :goods="showGoods"></goods-list>
    </scroll>

    <back-top @click.native="backClick" v-show="isShowBackTop"></back-top>
  </div>
</template>

<script>
import NavBar from "components/common/navbar/NavBar";
import Scroll from "components/common/scroll/Scroll";

import TabControl from "components/content/tabControl/TabControl";
import GoodsList from "components/content/goods/GoodsList";
import BackTop from "components/content/backTop/BackTop";

import HomeSwiper from "./childComps/HomeSwiper";
import RecommendView from "./childComps/RecommendView";
import FeatureView from "./childComps/FeatureView";

import { getHomeMultidata, getHomeGoods } from "network/home";
import { debounce } from "common/utills";

export default {
  name: "Home",
  data() {
    return {
      banners: [],
      recommends: [],
      goods: {
        pop: { page: 0, list: [] },
        new: { page: 0, list: [] },
        sell: { page: 0, list: [] },
      },
      currentType: "pop",
      isShowBackTop: false,
      tabOffsetTop: 0,
      isTabFixed: false,
      saveY: 0,
    };
  },
  components: {
    NavBar,
    Scroll,

    TabControl,
    GoodsList,
    BackTop,

    HomeSwiper,
    RecommendView,
    FeatureView,
  },
  created() {
    // 1. 请求多个数据
    this.homeMultidata();
    // 2. 请求商品数据
    this.homeGoods("pop");
    this.homeGoods("new");
    this.homeGoods("sell");
  },
  mounted() {
    // 1. 监听item中图片加载完成
    const refresh = debounce(this.$refs.scroll.refresh, 50);
    this.$bus.$on("itemImageLoad", () => {
      refresh();
    });
    // this.$bus.$on("itemImageLoad", () => {
    //   this.$refs.scroll && this.$refs.scroll.refresh();
    // });
  },
  methods: {
    /* 网络请求相关方法 */
    // 请求轮播图相关数据
    homeMultidata() {
      getHomeMultidata().then((res) => {
        this.banners = res.data.banner.list;
        this.recommends = res.data.recommend.list;
      });
    },
    // 请求商品数据
    homeGoods(type) {
      const page = this.goods[type].page + 1;
      getHomeGoods(type, page).then((res) => {
        this.goods[type].list.push(...res.data.list);
        this.goods[type].page += 1;

        // 完成上拉加载更多
        this.$refs.scroll && this.$refs.scroll.finishPullUp();
      });
    },

    /* 事件监听相关 */
    tabClick(index) {
      this.currentType = Object.keys(this.goods)[index];
      this.$refs.tabControl1.currentIndex = index
      this.$refs.tabControl2.currentIndex = index
      // 每次点击切换栏回到列表顶部
      this.$refs.scroll.scrollTo(0, -this.tabOffsetTop, 0)
    },
    // 点击返回顶部
    backClick() {
      this.$refs.scroll.scrollTo(0, 0);
    },
    // 返回顶部及tabControl栏的状态
    contentScroll(position) {
      // 1. 判断 backTop 是否显示
      this.isShowBackTop = position.y < -1000;

      // 2. 决定 tabControl是否吸顶(position: fixed)
      this.isTabFixed = position.y <= -this.tabOffsetTop;
    },
    // 下拉加载更多
    loadMore() {
      this.homeGoods(this.currentType);
      // this.$refs.scroll.scroll.refresh()
    },
    // 监听轮播图是否加载成功, 获取tabcontrol定位距离
    swiperImageLoad() {
      // 2. 获取 tabControl 的 offsetTop
      this.tabOffsetTop = this.$refs.tabControl2.$el.offsetTop;
    },
  },
  computed: {
    showGoods() {
      return this.goods[this.currentType].list;
    },
  },
  // 回来时页面状态不变
  activated() {
    this.$refs.scroll.scrollTo(0, this.saveY, 0)
    this.$refs.scroll.refresh()
  },
  // 保存离开时页面滑动高度
  deactivated() {
    this.saveY = this.$refs.scroll.getScrollY()
  }
};
</script>

<style scoped>
  #home {
    height: 100vh;
    position: relative;
  }
  .home-nav {
    background-color: var(--color-tint);
    font-weight: 700;
    font-size: 18px;
    color: #fff;

    /* position: fixed;
                top: 0;
                left: 0;
                right: 0;
                z-index: 9; */
  }
  /* .content {
          margin-top: 44px;
          height: calc(100vh - 93px);
          overflow: hidden;
        } */
  .content {
    overflow: hidden;
    position: absolute;
    top: 44px;
    bottom: 49px;
    left: 0;
    right: 0;
  }
  .tab-control {
    position: relative;
    z-index: 9;
  }
</style>