<template>
  <div class="av">
      <Navheader />
      <!-- <v-tab :isSubTab="true" @refresh="param.page++" :tabs="tabs" @clickHandle="clickHandle">
        <div style="height: 100%" class="clearfix" @refresh="param.page++" v-loading="loading">
          <videos :video-item="item" v-for="(item, index) in showRes" :key="item.id+index" />
        </div>
      </v-tab> -->
      <Video :list='titlelist'/>
  </div>
</template>

<script>
import Video from '@/components/new/Video.vue'
import Navheader from '@/components/new/Navheader.vue'

export default {
  components: {
    Video,Navheader
  },
  data () {
    return {
      titlelist:{}
    }
  },
  computed: {
  },
  watch: {
  },
  methods: {
    // 获取分类
    getCategory () {
      this.$http.get('/mapi/category/getvediolist').then(res => {
        if (res.status === 0) {
           console.log(res.data)
           this.titlelist=res.data
        }
      })
    },
    // // 在线广告
    // Onlineadvertising(){
    //   this.$http.get('/api/advert/list',{cate_code:'WebVideoList'}).then(res => {
    //     if (res.status === 0) {
    //       this.WebVideoList = res.data
    //     }
    //   })
    // },
    // 获取视频列表
    getlist() {
      let param =  {
        categoryid: 2,
        page: 1,
        page_size: 10
      }
      this.$http.get('/mapi/category/categorydetail', param).then(res => {
        if (res.status === 0) {
          // 插入广告
          let hotlist=res.data.hot
          let newlist=res.data.new
        }
      })
    }
  },
  created () {
    this.getlist()
    this.getCategory()
  }
}
</script>

<style scoped>
</style>
