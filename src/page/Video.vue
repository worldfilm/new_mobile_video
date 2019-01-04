<template>
  <div class="Video">
    <Navheader :title="title"/>
    <div class="swiper-container">
      <div class="swiper-wrapper">
        <div class="swiper-slide">
          <Tab/>
          <VideoList :list="VList"/>
        </div>
      </div>
    </div>
    <Footer/>
  </div>
</template>
<script>
import Hub from '@/components/Hub.vue'
import vTab from '@/components/tabs.vue'
import VideoList from '@/components/VideoList.vue'
import Navheader from '@/components/Navheader.vue'
import Footer from '@/components/Footer.vue'
import Tab from '@/components/new/Tab.vue'
import Swiper from 'swiper';
import 'swiper/dist/css/swiper.min.css';
export default {
  components: {
    Navheader,
    Tab,Footer,VideoList
  },
  data () {
    return {
      loading: false,
      // titlelist:[{name: '最新更新',id: 7},{name: '最热视频',id: 9},{name: '自拍偷拍',id: 24}],
      VList:[],
      title:"Video"
    }
  },
  methods: {
    // get(id){
    //   console.log(id)
    // },
    // 获取分类
    // getTabList() {
    //   this.$http.get('/mapi/category/getvediolist').then(res => {
    //     if (res.status === 0) {
    //     }
    //   })
    // },
    // 获取视频列表
    // getVideoList(id) {
    //   let param = {
    //     categoryid: id,
    //     page: 1,
    //     page_size: 10
    //   }
    //   this.$http.get('/mapi/category/categorydetail', param).then(res => {
    //     this.loading = false
    //     if (res.status === 0) {
    //       // Hub.$emit('titlelist',this.titlelist)
    //       this.VList=res.data
    //       // Hub.$emit('VList',this.VList)
    //     }
    //   })
    // },
    // getID(){
    //   Hub.$on('Navheader', data=>{
    //     this.getVideoList(data)
    //   })
    // }
  },
  mounted() {
    var mySwiper = new Swiper('.swiper-container', {
      scrollbar: '.swiper-scrollbar',
      direction: 'vertical',
      slidesPerView: 'auto',
      freeMode: true
    })
  },
  created () {
    // this.get()
    // this.getVideoList(2)// 获取视频列表
    // this.getTabList()// 获取分类
    // Hub.$emit('titlelist','Video')
    // this.getID()
    // Hub.$emit('titlelist',[{name: '最新更新',id: 7},{name: '最热视频',id: 9},{name: '自拍偷拍',id: 24}])
  }
}
</script>
<style lang="scss" scoped>
.Video {
      margin-top: 1rem;
    .swiper-container {
        z-index: 0;
    }
}
</style>
