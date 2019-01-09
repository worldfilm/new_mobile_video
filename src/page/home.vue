<template>
  <div class="home" >
    <HeadeNav/>
      <div class="swiper-container" :style="{'margin-top':marginData}">
        <div class="swiper-wrapper">
          <div class="swiper-slide"><AV/></div>
          <div class="swiper-slide"><Video/></div>
          <div class="swiper-slide"><Tag/></div>
          <!-- <div class="swiper-slide">'Tag'</div> -->
          <div class="swiper-slide"><Collect/></div>
          <div class="swiper-slide"><User/></div>
        </div>
      </div>
      <Footer/>
  </div>
</template>
<script>
import Footer from '@/components/Footer.vue'
import HeadeNav from '@/components/HeadeNav.vue'
import Hub from '@/components/Hub.vue'
import AV from '@/page/AV.vue'
import Video from '@/page/Video.vue'
import Tag from '@/page/Tag.vue'
import Collect from '@/page/Collect.vue'
import User from '@/page/user/index.vue'
import Swiper from 'swiper';
import 'swiper/dist/css/swiper.min.css';
export default {
  components: {
    AV,Video,Tag,Collect,User,Footer,HeadeNav
  },
  data () {
    return {
       widthData:0,
       heightData:0,
       activeVal:0,
       displayData:'',
       ShowFooter:'',
       marginData:'0.8rem',
    }
  },
  mounted(){
       var mySwiper = new Swiper('.swiper-container', {
         autoplay: false
       })
      mySwiper.on('slideChangeTransitionEnd', function () {
       let activeIndex=mySwiper.activeIndex
       // console.log(activeIndex)
       if(activeIndex==4){
         console.log(activeIndex)
         Hub.$emit('ShowHeadeNav', false);
         Hub.$emit('ShowFooter', false);
       }else{
         Hub.$emit('ShowHeadeNav', true);
         Hub.$emit('ShowFooter', true);
         // this.marginData='0.8rem'
       }
       Hub.$emit('sendingIdx', activeIndex);
      });

    },
    created () {
      this.widthData=document.documentElement.clientWidth
      this.heightData=document.documentElement.clientHeight
      this.ShowHeadeNav=true
      this.ShowFooter=true
      Hub.$on('marginData', data => {
          this.marginData=data
      });
    }
}
</script>
<style lang="scss" scoped>
.swiper-container {
  width: 100%;
  // height: 87%;
  z-index: 0;
  margin-bottom: 1rem;
}
.swiper-slide {
  text-align: center;
  font-size: 18px;
  background: #fff;
  display: -webkit-box;
  display: -ms-flexbox;
  display: -webkit-flex;
  display: flex;
  -webkit-box-pack: center;
  -ms-flex-pack: center;
  -webkit-justify-content: center;
  justify-content: center;
  -webkit-box-align: center;
  -ms-flex-align: center;
  -webkit-align-items: center;
  align-items: center;
}
</style>
