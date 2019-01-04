<template>
  <div class="VideoList">
    <ul class="cube-slide">
      <li v-for='item in list'  @click="toDetail(item.id)">
        <img v-lazy="item.thumb_img_url" :key="item.thumb_img_url" />
        <span class="iconfont icon-shoucang like"  @click="collect(item.id)" ></span>
        <div class="time" v-text="item.created_at"></div>
        <span class="title" v-text='item.title'></span>
      </li>
    </ul>
  </div>
</template>
<!-- 插入广告 -->
<!-- <a :href="totalList.url" target="view_window">
  <div class="row1" v-if='ad'>
    <img class="rowImg" :src="totalList.img_url"/>
    <span class="rowTitle">{{totalList.title}}</span>
  </div>
</a> -->
<!-- 插入广告 -->
<script>
import Hub from '@/components/Hub.vue'
import BScroll from 'better-scroll'
export default {
  props:["VList"],
  data () {
    return {
      isCollected: false,
      defaultImg:"this.src='/default1.jpg'",
      ad:false,
      mainlist:[],
      list:[],
      id:2,
      tabnum:0,
    }
  },
  methods: {
     collect(id){},
     toDetail(id){
       this.$router.push({path: `/videoPlay/${id}`})
     },
     // // 获取视频列表
     // getVideoList(id) {
     //   let param =  {
     //     categoryid: id,
     //     page: 1,
     //     page_size: 10
     //   }
     //   this.$http.get('/mapi/category/categorydetail', param).then(res => {
     //     if (res.status === 0) {
     //       this.mainlist=res.data
     //       if(this.tabnum==0){
     //         this.list=this.mainlist.new
     //       }
     //       if(this.tabnum==1){
     //         this.list=this.mainlist.hot
     //       }
     //     }
     //   })
     // },
     getID(){
       Hub.$on('Navheader', data=>{
         this.getVideoList(data)
       })
     },
     scrollFn () {
         if (!this.scroll) {
           this.scroll = new BScroll(this.$refs.bscroll, {
             startY: 0,
             click: true,
             scrollY: true,
             probeType: 2,
             tap: true
           })
         } else {
           this.scroll.refresh()
         }
         if (this.isScroll) {
           /**
            * 控制导航条显示隐藏
            */
           let startY = 0
           // 滚动事件
           this.scroll.on('scroll', pos => {
             if (startY > pos.y) {
               this.show = false
             } else {
               this.show = true
             }
             startY = pos.y
           })
           // touchEnd（手指离开以后触发） 通过这个方法来监听下拉刷新
           this.scroll.on('touchEnd', (pos) => {
             // 上拉加载 总高度>下拉的高度+10 触发加载更多
             if (this.scroll.maxScrollY > pos.y + 40) {
               // 使用refresh 方法 来更新scroll  解决无法滚动的问题
               this.dropUp = true
               this.$emit('refresh')
               this.scroll.refresh()
             }
           })
         }
     }
  },
  // beforeMount() {
  //   console.log('beforeMount')
  // },
  mounted() {
    // console.log(this.VList)
    // this.getID()
  },
  // beforeUpdate() {
  //   console.log('beforeUpdate')
  // },
  // destroyed() {
  //   this.getVideoList()
  //   console.log('updated')
  // },
  // beforeDestroy() {
  //   console.log('beforeDestroy')
  // },
  // destroyed() {
  //   console.log('destroyed')
  // },
  created () {
console.log(this.VList)
     // console.log('created')
  }
}
</script>

<style lang="scss" scoped>
.VideoList {
  // padding-top: 1rem;
  ul{
    li{
       display: inline-block;
       width:49%;
       float: left;
       margin: 0.5%;
       position: relative;
       // height: 2.7rem;
       font-size: 0.2rem;
       .like {
         position: absolute;
         top: 0.2rem;
         right: 0.2rem;
         padding: 0.08rem;
         font-size: 0.5rem;
         color: pink;
         background: rgba($color: #000, $alpha: 0.5);
         border-radius: 50%;
       }
       .title{
         height:0.5rem;
         line-height:0.5rem;
         padding-left: 0.2rem;
         width: 3.66rem;
         font-size: 0.2rem;
         float: left;
         overflow: hidden;
         text-overflow: ellipsis;
         white-space: nowrap;
       }
       img{
         width: 100%;
         height: 2.3rem;
       }
       .time {
         position: absolute;
         bottom: 0.5rem;
         left: 0;
         width: 100%;
         height: 0.5rem;
         padding-left: 0.1rem;
         color: #fff;
         line-height: 0.5rem;
         text-align: left;
         background: rgba($color: #000000, $alpha: 0.3);
       }
    }
  }
}
</style>
