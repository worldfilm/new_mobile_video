<template>
  <div class="Collect">
    <v-tab :tabs="tabs" @clickHandle="clickHandle">
      <div v-loading="loading" style="height: 100%;">
        <div class="actors-container">
          <actor-item v-if="tabId === '0'" :actor-info="item" v-for="(item, index) in actors" class="act-item" :key="index" @click="toDetail(item)"/>
          <VideoList v-if="['1','2'].indexOf(tabId) > -1"  :show-collect="false" :video-item="item" v-for="(item) in videos" :key="item.id"/>
        </div>
      </div>
    </v-tab>
  </div>
</template>

<script>
import vTab from '@/components/tabs.vue'
import VideoList from '@/components/VideoList.vue'
import actorItem from '@/components/actorItem.vue'
import Footer from '@/components/Footer.vue'
const tabs = [
  {
    label: '女优',
    id: '0'
  },
  {
    label: 'AV',
    id: '1'
  },
  {
    label: '视频',
    id: '2'
  }
]
export default {
  components: {
    vTab,
    VideoList,
    actorItem,
    Footer
  },
  data () {
    return {
      loading: false,
      tabs,
      tabId: '0',
      params: {
        page: 1,
        page_size: 12,
        video_category: 1
      },
      actors: [],
      videos: []
    }
  },
  created(){

  },
  methods: {
    clickHandle (id) {
      this.tabId = id
      if (id == 0) {
        this.getCollectActor ()
      }else{
        this.getCollect(id)
      }
    },
    toDetail (item) {
      this.$router.push({path: '/actorDetail', query: {actorId: item.actorId}})
    },
    getCollect (id) {
      this.loading = true
      if (id) {
        this.params.video_category = id
      }
      const params = this.params
      this.$http.get('/api/user/list/collect', params).then(res => {
        this.loading = false
        if (res.status === 0) {
          let list =res.data.list
          for(var i in list){
              list[i].local_head_url=this.$MyConfig.baseURL+list[i].local_head_url
          }
          this.actors = list
          this.videos = res.data.list
        }
      })
    },
    getCollectActor () {
      this.loading = true
      this.$http.get('/mapi/category/getCollectActor').then(res => {
        this.loading = false
        if (res.status === 0) {
          let list =res.data.list
          for(var i in list){
              list[i].local_head_url=this.$MyConfig.baseURL+list[i].local_head_url
          }
          this.actors = list
        }
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.Collect{
  // width: 8rem;
// height: 20rem;
}
.actors-container {
  margin-top: 0.2rem;
  flex-wrap: wrap;
  .act-item {
    width: 25%;
    margin-bottom: 0.2rem;
    text-align: center;
    display: inline-block;
    .name {
      font-size: 0.3rem;
      line-height: 0.6rem;
    }
    .avator {
      width: 1.5rem;
      height: 1.5rem;
      border-radius: 50%;
      display: inline-block;
    }
  }
}
</style>
