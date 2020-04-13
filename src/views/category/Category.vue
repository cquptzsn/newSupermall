<template>
  <div>
    <div class="wrapper">
      <ul class="content">
        <button @click="btnClick">按钮</button>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
        <li>1111111</li>
      </ul>
    </div>
  </div>

</template>

<script>

  import BScroll from 'better-scroll'


  export default {
    name: "Category",
    data(){
      return {
        scroll: null
      }
    },
    mounted(){
      //console.log(document.querySelector('.wrapper'))

      this.scroll = new BScroll(document.querySelector('.wrapper'), {
        probeType: 3,
        pullUpLoad: true
      })

      this.scroll.on('scroll', (position) => {
        console.log(position)
      })

      this.scroll.on('pullingUp', () => {
        console.log('上拉加载更多')

        this.scroll.finishPullUp()
      })
    },
    methods: {
      btnClick(){
        console.log('按钮被点击了')
      }
    }
  }
</script>

<style scoped>
  .wrapper {
    height: 160px;
    background-color: #ff5777;

    overflow: hidden;

    /*overflow-y: scroll;*/
    /*超出部分会自动隐藏*/
  }
</style>
