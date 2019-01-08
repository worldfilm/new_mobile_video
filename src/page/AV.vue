<template>
  <div class="AV">
      <v-tab :isSubTab="true" @refresh="param.page++" :tabs="tabs" @clickHandle="clickHandle">
        <div style="height: 100%" class="clearfix" @refresh="param.page++" v-loading="loading">
          <VideoList :video-item="item" v-for="(item, index) in showRes" :key="item.id+index" />
        </div>
      </v-tab>
  </div>
</template>

<script>
import vTab from '@/components/tabs.vue'
import VideoList from '@/components/VideoList.vue'
import Footer from '@/components/Footer.vue'

export default {
  components: {
    vTab,
    VideoList,Footer
  },
  data () {
    return {
      hotListData:[],
      WebVideoList:[],
      tabs: [],
      videos: {
        hot: [],
        new: []
      },
      param: {
        categoryid: null,
        page: 1,
        page_size: 10
      },
      resStr: 'new',
      loading: false
    }
  },
  computed: {
    showRes () {
      // debugger
      return this.videos[this.resStr] || []
    }
  },
  watch: {
    'param.page' () {
      this.getVideosByCategory()
    },
    'param.categoryid' () {
      this.loading = true
      this.resStr = 'new'
      this.videos = {
        hot: [],
        new: []
      }
      this.getVideosByCategory()
    },
    resStr () {
      this.videos = {
        hot: [],
        new: []
      }
      this.param.page === 1 ? this.getVideosByCategory() : this.param.page = 1
    }
  },
  methods: {
    clickHandle (id, str) {
      this.resStr = str
      this.param.categoryid = id
      this.param.page = 1
    },
    // 获取分类
    getCategory () {
      this.$http.get('/mapi/category/getvediolist').then(res => {
        if (res.status === 0) {
          this.tabs = res.data.map(item => {
            return {
              id: item.id,
              label: item.name
            }
          })
          this.param.categoryid = this.tabs[0].id
        }
      })
    },
    // 在线广告
    Onlineadvertising(){
      this.$http.get('/api/advert/list',{cate_code:'AppVideoList'}).then(res => {
        if (res.status === 0) {
          this.WebVideoList = res.data
        }
      })
    },
    // 获取视频列表
    getVideosByCategory () {
      let param = this.param
      this.$http.get('/mapi/category/categorydetail', param).then(res => {
        if (res.status === 0) {
          // 插入广告
          let hotlist=res.data.hot
          let newlist=res.data.new
          for(var i in hotlist){
            if(i / 5 >= 1 && i % 5 == 0){
              hotlist[i].xxx = this.WebVideoList[this.param.page -1]
              this.hotListData = hotlist[i].xxx
            }
          }
          if(this.hotListData === undefined){
            } else{
              window.sessionStorage.setItem('hotListData', JSON.stringify(this.hotListData))
          }
          for(var i in newlist){
            if(i / 5 >= 1 && i % 5 == 0){
              newlist[i].xxx = this.WebVideoList[this.param.page -1]
              this.hotListData = newlist[i].xxx
            }
          }
          if(this.hotListData === undefined){
            } else{
              window.sessionStorage.setItem('hotListData', JSON.stringify(this.hotListData))
          }
          res.data={'new':newlist,'hot':hotlist}
          // debugger
          // 插入广告
          this.videos[this.resStr] = this.videos[this.resStr].concat(res.data[this.resStr])
          this.$children[0].dropDown = false
          this.$children[0].dropUp = false
          this.loading = false
        }
      })
    }
  },
  created () {
    this.getCategory()
    this.Onlineadvertising()
  }
}
</script>

<style scoped>
.AV{
  /* width: 8rem; */
/* height: 20rem; */
}
</style>
