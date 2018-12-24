<template>
  <div class="Video">
    <Navheader/>
    <div class="swiper-container">
      <div class="swiper-wrapper">
        <div class="swiper-slide">
          <Tab/>
          <VideoList/>
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

export default {
  components: {
    Navheader,
    Tab,Footer,VideoList
  },
  data () {
    return {
      videos: {
        new: [],
        hot: []
      },
      showRes: [],
      str: 'new',
      param: {
        categoryid: '',
        page: 1,
        page_size: 10
      },
      loading: false,
      titlelist:[{name: '最新更新',id: 7},{name: '最热视频',id: 9},{name: '自拍偷拍',id: 24}],
    }
  },
  watch: {
    'param.page' () {
      this.getVideosByCategory(this.param.categoryid)
    }
  },
  methods: {
    clickHandle (str) {
      this.showRes = []
      this.param.page = 1
      if (str === 'new' || str === 'hot') {
        this.str = str
        this.getVideosByCategory()
        return
      } else if (str === '7,9,24'){
        this.getVideosByCategory(str)
      }
    },
    // 获取视频列表
    getVideosByCategory (id = '') {
      this.loading = true
      this.param.categoryid = id
      let param = this.param
      this.$http.get('/mapi/category/categorydetail', param).then(res => {
        this.loading = false
        if (res.status === 0) {
          this.$children[0].dropUp = false
          if (id) {
            this.showRes = this.showRes.concat(res.data.new)
          } else {
            this.showRes = this.showRes.concat(res.data[this.str])
          }
        }
      })
    },
    // 获取分类
    getTabList() {
      this.$http.get('/mapi/category/getvediolist').then(res => {
        if (res.status === 0) {
           Hub.$emit('titlelist',this.titlelist)
        }
      })
    },
  },
  created () {
    this.getTabList()
    this.clickHandle()
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
