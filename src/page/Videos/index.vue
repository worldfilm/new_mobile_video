<template>
  <div class="videos">
    <v-tab :tabs="tabs" @clickHandle="clickHandle" @refresh="param.page++">
      <div style="height: 100%" class="clearfix" v-loading="loading">
        <videos :video-item="item" v-for="(item, index) in showRes" :key="item.id + index" />
      </div>
    </v-tab>
  </div>
</template>

<script>
import vTab from '@/components/tabs.vue'
import videos from '@/components/videos.vue'
const tabs = [
  {
    label: '最新更新',
    id: 'new'
  },
  {
    label: '最热视频',
    id: 'hot'
  },
  {
    label: '自拍偷拍',
    id: '7,9,24'
  }
]
export default {
  components: {
    vTab,
    videos
  },
  data () {
    return {
      tabs,
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
      loading: false
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
    }
  },
  created () {
    this.clickHandle()
  }
}
</script>

<style scoped>

</style>
