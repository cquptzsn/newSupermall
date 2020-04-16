<template>
  <div class="wrapper" ref="wrapper">
    <div class="content">
      <slot></slot>
    </div>
  </div>
</template>

<script>
  import BScroll from 'better-scroll'

  export default {
    name: "scroll",
    data(){
      return {
        scroll: null
      }
    },
    props: {
      probeType: {
        type: Number,
        default: 0
      },
      pullUpLoad: {
        type: Boolean,
        default: false
      }

    },
    mounted(){
      //.创建BScroll对象
      this.scroll = new BScroll(this.$refs.wrapper, {
        click: true,//设置元素是否可以点击
        probeType: this.probeType,//设置是否监听滚动
        pullUpLoad:this.pullUpLoad//设置是否上拉加载更多
      });

      //2.监听滚动位置
      this.scroll.on('scroll', (position) => {
        //console.log(position)
        this.$emit('scroll', position)//把滚动事件发送出去，参数为position
      });

      //3.监听上拉加载更多
      this.scroll.on('pullingUp', () => {
        this.$emit('pullingUp');
        this.scroll.finishPullUp()//结束上拉加载更多，不然只能加载一次
      })
    },
    methods: {
      refresh(){
        this.scroll && this.scroll.refresh()
      },//对refresh函数的稍微封装，方便使用
    }
  }
</script>

<style scoped>

</style>
