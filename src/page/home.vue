<template>
  <div class="home" :style="{width:widthData+'px',height:heightData+'px'}">
    <div class="container">
      <div class="swiper-container">
         <div class="swiper-wrapper">
            <div class="swiper-slide">
              <AV/>
            </div>
            <div class="swiper-slide">
              <Video/>
            </div>
            <div class="swiper-slide">
              <Tag/>
            </div>
            <div class="swiper-slide">
              <Collect/>
            </div>
            <div class="swiper-slide">
              <User/>
            </div>
         </div>
      </div>
    </div>
  </div>
</template>

<script>
import AV from '@/page/AV.vue'
import Video from '@/page/Video.vue'
import Tag from '@/page/Tag.vue'
import Collect from '@/page/Collect.vue'
import User from '@/page/user/index.vue'
import Swiper from 'swiper';
import 'swiper/dist/css/swiper.min.css';
export default {
  components: {
    AV,Video,Tag,Collect,User
  },
  data () {
    return {
       ClientWidth:0,
       ClientHeight:0,
       widthData:0,
       heightData:0
    }
  },
  mounted(){
       var mySwiper = new Swiper('.swiper-container', {
         autoplay: false
       })
       let ClientWidth=document.documentElement.clientWidth //滚动条高度
       let ClientHeight=document.documentElement.clientHeight//浏览器高度
       this.ClientWidth=ClientWidth
       this.ClientHeight=ClientHeight
       this.widthData=ClientWidth
       this.heightData=ClientHeight
    },
    created () {
      let ClientWidth=document.documentElement.clientWidth //滚动条高度
      let ClientHeight=document.documentElement.clientHeight//浏览器高度
      this.ClientWidth=ClientWidth
      this.ClientHeight=ClientHeight
      this.widthData=ClientWidth
      this.heightData=ClientHeight
    }
}
</script>
