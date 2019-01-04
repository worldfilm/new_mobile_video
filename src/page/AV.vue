<template>
<div class="AV">
  <Navheader :title='title'/>
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
import VideoList from '@/components/VideoList.vue'
import Navheader from '@/components/Navheader.vue'
import Footer from '@/components/Footer.vue'
import Tab from '@/components/new/Tab.vue'
import Swiper from 'swiper';
import 'swiper/dist/css/swiper.min.css';
export default {
  components: {
    VideoList,
    Navheader,
    Tab,
    Footer
  },
  data() {
    return {
      titlelist:{},
      VList:[],
      title:"AV"
    }
  },
  computed: {},
  watch: {},
  methods: {
    // 获取分类
    // getTabList() {
    //   this.$http.get('/mapi/category/getvediolist').then(res => {
    //     if (res.status === 0) {
    //        this.titlelist=res.data
    //     }
    //   })
    // },
    // // 在线广告
    // Onlineadvertising(){
    //   this.$http.get('/api/advert/list',{cate_code:'WebVideoList'}).then(res => {
    //     if (res.status === 0) {
    //       this.WebVideoList = res.data
    //     }
    //   })
    // },
    // 获取视频列表
    getVideoList(id) {
      let param = {
        categoryid: id,
        page: 1,
        page_size: 10
      }
      this.$http.get('/mapi/category/categorydetail', param).then(res => {
        if (res.status === 0) {
          this.VList=res.data
          // Hub.$emit('VList',this.VList)
          // this.mainlist = res.data
          // if (this.tabnum == 0) {
          //   this.list = this.mainlist.new
          // }
          // if (this.tabnum == 1) {
          //   this.list = this.mainlist.hot
          // }
          // 插入广告
          // let hotlist=res.data.hot
          // let newlist=res.data
        }
      })
    },
    getID(){
      Hub.$on('Navheader', data=>{
        this.getVideoList(data)
      })
    }
  },
  mounted() {
    var mySwiper = new Swiper('.swiper-container', {
      scrollbar: '.swiper-scrollbar',
      direction: 'vertical',
      slidesPerView: 'auto',
      freeMode: true
    })

  },
  created() {
    this.getVideoList(2)// 获取视频列表
    // this.getTabList()// 获取分类
    // Hub.$emit('titlelist','AV')
    // this.getID()
    Hub.$on('Navheader', data=>{
      this.getVideoList(data)
    })
  }
}
</script>
<style lang="scss" scoped>
.AV {
      margin-top: 1rem;
    .swiper-container {
        z-index: 0;

    }
}
</style>
