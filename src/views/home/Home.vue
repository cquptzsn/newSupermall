<template>
  <div id="home">
    <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>

    <tab-control :titles="['流行','新款','精选']"
                 @tabClick="tabClick"
                 ref="tabControl1"
                 class="tab-control" v-show="isTabFixed"></tab-control>

    <scroll class="content"
            ref="scroll"
            :probe-type="3"
            @scroll="contentScroll"
            :pull-up-load="true"
            @pullingUp="loadMore">
      <home-swiper :banners="banners" @swiperImageLoad="swiperImageLoad"></home-swiper>
      <recommend-view :recommends="recommends"></recommend-view>
      <feature-view/>
      <tab-control class="tab-control"
                   :titles="['流行','新款','精选']"
                   @tabClick="tabClick"
                   ref="tabControl2"></tab-control>
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
import {getHomeGoods} from "network/home"
import {debounce} from "common/utils/utils";


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
        isShowBackTop: false,
        tabOffsetTop:0,//滚动了多少距离后tabControl有吸顶效果
        isTabFixed: false,
        scrollY:0 //保存离开时页面的位置
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

    mounted(){
      //防抖之前，会打印30次，刷新30次
      // this.$bus.$on('goodsItem-imageLoad', () => {
      //   this.$refs.scroll.refresh()
      // })

      //防抖后，刷新次数减少，提高性能
      const refresh = debounce(this.$refs.scroll.refresh, 100)
      //3.监听goodsItem中图片加载完成，然后刷新scrollerHeight
      this.$bus.$on('goodsItem-imageLoad', () => {
        refresh()
      })

    },

    activated(){
      this.$refs.scroll.refresh()
      this.$refs.scroll.scroll.scrollTo(0, this.scrollY, 0)

    },

    deactivated(){
      this.scrollY = this.$refs.scroll.scroll.y
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
        this.$refs.tabControl1.currentIndex = index;
        this.$refs.tabControl2.currentIndex = index;
      },//tabControl按钮点击切换 流行、新款、精选

      backClick(){
        this.$refs.scroll.scroll.scrollTo(0, 0, 500);
      },//通过$refs得到scroll组件然后直接访问scroll组件里面的scroll对象，scroll对象直接调用scrollTo方法回到顶部

      contentScroll(position){
        //1.根据position的值监听是否能显示backTop
        if (position.y < -1000){
          this.isShowBackTop = true
        }
        if (position.y > -1000){
          this.isShowBackTop = false
        }

        //2.根据position的值动态设置tabControl的样式
        this.isTabFixed = -(position.y) > this.tabOffsetTop

      },//根据 position.y 实时判断返回顶部图标是否显示

      loadMore(){
        this.getHomeGoods(this.currentType)
      },//调用getHomeGoods完成上拉加载更多

      swiperImageLoad(){
        //获取tabControl的offsetTop值
        //通过$el 获取组件的元素,每一个元素都有一个offsetTop值
        this.tabOffsetTop = this.$refs.tabControl2.$el.offsetTop;

      },

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
    height: 100vh;
    position: relative;
  }

  .home-nav {
    background-color: var(--color-tint);
    color: #fff;


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

  .tab-control {
    position: relative;
    z-index: 9;
  }




</style>
