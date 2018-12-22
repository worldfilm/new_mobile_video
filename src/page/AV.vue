<template>
  <div class="av">
      <Navheader/>
      <Tab/>
      <Video/>
  </div>
</template>
<script>
import Video from '@/components/new/Video.vue'
import Navheader from '@/components/new/Navheader.vue'
import Tab from '@/components/new/Tab.vue'
export default {
  components: {
    Video,Navheader,Tab
  },
  data () {
    return {
      // titlelist:{}
    }
  },
  computed: {
  },
  watch: {
  },
  methods: {
    // 获取分类
    // getCategory () {
    //   this.$http.get('/mapi/category/getvediolist').then(res => {
    //     if (res.status === 0) {
    //        console.log(res.data)
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

  },
  mounted () {
    // this.getlist()
      // this.getCategory()
  },
  created () {
    // this.getlist()
    // this.getCategory()
  }
}
</script>

<style scoped>
</style>
