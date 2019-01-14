<template>
  <div class="Video">
    <!-- <v-tab :tabs="tabs" @clickHandle="clickHandle" @refresh="param.page++"> -->
      <!-- <div style="height: 100%" class="clearfix" v-loading="loading"> -->
        <!-- <VideoList :video-item="item" v-for="(item, index) in showRes"/> -->
      <!-- </div> -->
    <!-- </v-tab> -->
  </div>
</template>

<script>
import vTab from '@/components/tabs.vue'
import VideoList from '@/components/VideoList.vue'
export default {
  components: {
    vTab,
    VideoList
  },
  data () {
    return {
      // tabs,
      // videos: {
      //   new: [],
      //   hot: []
      // },
      showRes: [],
      // str: 'new',
      param: {
        categoryid: 2,
        page: 1,
        page_size: 10
      },
      // loading: false
    }
  },
  // watch: {
  //   'param.page' () {
  //     this.getVideosByCategory(this.param.categoryid)
  //   }
  // },
  methods: {
    // clickHandle (str) {
    //   this.showRes = []
    //   this.param.page = 1
    //   if (str === 'new' || str === 'hot') {
    //     this.str = str
    //     this.getVideosByCategory()
    //     return
    //   } else if (str === '7,9,24'){
    //     this.getVideosByCategory(str)
    //   }
    // },
    // 获取视频列表
    getVideosByCategory (id = '') {
      // this.loading = true
      // this.param.categoryid = id
      let param = this.param

      this.$http.get('/mapi/category/categorydetail',param).then(res => {
        // this.loading = false
        if (res.status === 0) {
          // this.$children[0].dropUp = false
          // if (id) {
            this.showRes = this.showRes.concat(res.data.new)
          // } else {
          //   this.showRes = this.showRes.concat(res.data[this.str])
          // }
        }
      })
    }
  },
  created () {
    this.getVideosByCategory()
  }
}
</script>

<style scoped>
.Video{
  /* width: 8rem; */
/* height: 20rem; */
}
</style>
