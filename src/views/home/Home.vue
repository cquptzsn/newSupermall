<template>
  <div id="home">
    <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>

    <scroll class="content"
            ref="scroll"
            :probe-type="3"
            @scroll="contentScroll"
            :pull-up-load="true"
            @pullingUp="loadMore">
      <home-swiper :banners="banners"></home-swiper>
      <recommend-view :recommends="recommends"></recommend-view>
      <feature-view/>
      <tab-control class="tab-control"
                   :titles="['流行','新款','精选']"
                   @tabClick="tabClick"></tab-control>
      <goods-list :goods="goods[currentType].list"></goods-list>
    </scroll>

    <back-top @click.native="backClick" v-show="isShowBackTop"></back-top>



  </div>

</template>

<script>

import HomeSwiper from './childrenComps/HomeSwiper'
import RecommendView from './childrenComps/RecommendView'
import FeatureView from './childrenComps/FeatureView'


import NavBar from 'components/common/navbar/NavBar.vue'
import TabControl from 'components/content/tabbarControl/TabControl'
import GoodsList from 'components/content/goods/GoodsList'
import Scroll from 'components/common/scroll/Scroll'
import BackTop from 'components/content/backTop/BackTop'

import {getHomeMultidata} from 'network/home'
import {getHomeGoods} from "network/home";



export default {
    name: "Home",
    components: {
      Scroll,
      HomeSwiper,
      RecommendView,
      FeatureView,

      NavBar,
      TabControl,
      GoodsList,
      BackTop
    },
    data(){
      return {
        banners: [],
        recommends: [],
        goods: {
          'pop': {page: 0, list: []},
          'new': {page: 0, list: []},
          'sell': {page: 0, list: []}
        },
        currentType: 'pop',
        isShowBackTop: false
      }
    },

    created(){

      //1.请求轮播图，推荐等数据
      this.getHomeMultidata()

      //2.请求 流行、新款、精选的数据
      this.getHomeGoods('pop');
      this.getHomeGoods('new');
      this.getHomeGoods('sell');

    },

    methods: {
      /**
       *事件监听相关方法
       */
      tabClick(index){
        switch (index){
          case 0:
            this.currentType = 'pop'
            break
          case 1:
            this.currentType = 'new'
            break
          case 2:
            this.currentType = 'sell'
            break
        }
      },//tabControl按钮点击切换 流行、新款、精选

      backClick(){
        this.$refs.scroll.scroll.scrollTo(0, 0, 500);
      },//通过$refs得到scroll组件然后直接访问scroll组件里面的scroll对象，scroll对象直接调用scrollTo方法回到顶部

      contentScroll(position){
        if (position.y < -1000){
          this.isShowBackTop = true
        }
        if (position.y > -1000){
          this.isShowBackTop = false
        }
      },//根据 position.y 实时判断返回顶部图标是否显示

      loadMore(){
        this.getHomeGoods(this.currentType)
      },//调用getHomeGoods方法完成上拉加载更多

      /**
       * 网络请求相关方法
       */
      getHomeMultidata(){
        getHomeMultidata().then(res => {
          this.banners = res.data.banner.list;
          this.recommends = res.data.recommend.list
        })
      },
      getHomeGoods(type){
        const page = this.goods[type].page + 1
        getHomeGoods(type, page).then(res => {
          this.goods[type].list.push(...res.data.list)//保存数据
          this.goods[type].page +=1 //请求完一页数据之后页数page要+1
        })
      }

    }
  }
</script>

<style scoped>
  #home {
    padding-top: 44px;
    height: 100vh;
    position: relative;
  }

  .home-nav {
    background-color: var(--color-tint);
    color: #fff;

    position: fixed;/*固定首页导航栏*/
    left: 0;
    right: 0;
    top: 0;
    z-index: 9;
  }

  .tab-control {
    position: sticky;/*粘性定位*/
    top: 44px;/*粘性定位必须有一个top值来告诉在哪停留*/
    z-index: 9;
  }

  /*.content {*/
    /*height: calc(100% - 93px);*/
    /*overflow: hidden;*/
    /*margin-top: 44px;*/
  /*}*/

  .content {
    position: absolute;
    top: 44px;
    bottom: 49px;
    left: 0;
    right: 0;
    overflow: hidden;
  }
</style>
