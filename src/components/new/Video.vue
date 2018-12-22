<template>
  <div class="Video slide-wrapper" >
    <div class="swiper-container">
    <div class="swiper-wrapper">
      <div class="swiper-slide" >
        <ul class="cube-slide">
          <li v-for='item in list'  @click="toDetail(item.id)">
            <img v-lazy="item.thumb_img_url" :key="item.thumb_img_url" />
            <span class="iconfont icon-shoucang like"  @click="collect(item.id)" ></span>
            <div class="time"></div>
            <span class="title" v-text='item.title'></span>
          </li>
        </ul>
      </div>
    </div>
  </div>
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
import Swiper from 'swiper';
import 'swiper/dist/css/swiper.min.css';
export default {
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
     // 获取视频列表
     getlist(id) {
       let param =  {
         categoryid: id,
         page: 1,
         page_size: 10
       }
       this.$http.get('/mapi/category/categorydetail', param).then(res => {
         if (res.status === 0) {
           this.mainlist=res.data
           if(this.tabnum==0){
             this.list=this.mainlist.new
             // console.log(this.list)
           }
           if(this.tabnum==1){
             this.list=this.mainlist.hot
             // console.log(this.list)
           }
           // 插入广告
           // let hotlist=res.data.hot
           // let newlist=res.data
         }
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
  mounted(){
       var mySwiper = new Swiper('.swiper-container', {
         scrollbar: '.swiper-scrollbar',
         direction: 'vertical',
         slidesPerView: 'auto',
         freeMode: true
       })
     },
  created () {
    this.getlist(2)
    Hub.$on('Navheader',data=>{
      this.getlist(data)
    })
    Hub.$on('Tab',data=>{
        this.tabnum=data
        if(this.tabnum==0){
          this.list=this.mainlist.new
        }
        if(this.tabnum==1){
          this.list=this.mainlist.hot
        }
    })

  }
}
</script>

<style lang="scss" scoped>
.Video {
  padding-top: 1rem;
  ul{
    li{
       display: inline-block;
       width:49%;
       float: left;
       margin: 0.5%;
       position: relative;
       height: 3rem;
       overflow: hidden;
       text-overflow: ellipsis;
       white-space: nowrap;
       .like {
         position: absolute;
         top: 0.2rem;
         right: 0.2rem;
         padding: 0.08rem;
         font-size: 0.5rem;
         color: pink;
         // background: rgba($color: #000, $alpha: 0.5);
         border-radius: 50%;
       }
       .title{
         height:0.5rem;
         line-height:0.5rem;
         padding-left: 0.2rem;
       }
       img{
         width: 100%;
         height: 2.6rem;
       }
    }
  }
}
//
//   .list-item {
//     .img {
//       position: relative;
//       text-align: center;
//       img {
//         width: 3.7rem;
//         height:2.5rem;
//         // height: 3rem;
//       }
//       .time {
//         position: absolute;
//         bottom: 0;
//         left: 0;
//         width: 100%;
//         height: 0.5rem;
//         padding-left: 0.1rem;
//         color: #fff;
//         line-height: 0.5rem;
//         text-align: left;
//         background: rgba($color: #000000, $alpha: 0.3);
//       }
//       .like {
//         position: absolute;
//         top: 0.2rem;
//         right: 0.2rem;
//         padding: 0.08rem;
//         font-size: 0.5rem;
//         color: pink;
//         background: rgba($color: #000, $alpha: 0.5);
//         border-radius: 50%;
//       }
//       .liked {
//         background: rgba($color: #ec4e63, $alpha: 0.5);
//       }
//     }
//     .title {
//       width: 3.66rem;
//       font-size: 0.2rem;
//       height: 0.35rem;
//       overflow: hidden;
//       text-overflow: ellipsis;
//       white-space: nowrap;
//     }
//     .desc {
//       font-size: 0.3rem;
//       padding: 0 0.2rem;
//       color: #ccc;
//     }
//   }
//
//   .time {
//     font-size: 13px;
//     color: #999;
//   }
//   .image {
//     // width: 100%;
//     display: block;
//   }
//
//   .clearfix:before,
//   .clearfix:after {
//     display: table;
//     content: "";
//   }
//
//   .clearfix:after {
//     clear: both;
//   }
// }
</style>
