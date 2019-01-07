<template>
  <div class="my-videos">
    <HeaderBar>
      <router-link to="/upload">上传</router-link>
    </HeaderBar>
    <div class="video-box" v-loading="loading">
      <null-tips tip-text="你还没有上传过视频！" v-if="hasData"></null-tips>
      <VideoList :video-item="item" :show-collect="false" v-for="item in videos" :key="item.id" />
    </div>
  </div>
</template>

<script>
import HeaderBar from '@/components/HeaderBar.vue'
import VideoList from '@/components/VideoList.vue'
export default {
  components: {
    HeaderBar,
    VideoList
  },
  data () {
    return {
      videos: [],
      hasData: false,
      loading: false,
      params: {
        page: 1,
        page_size: 10,
      }
    }
  },
  methods: {
    getMyVideos () {
      let params = this.params
      this.loading = true
      this.$http.get('/api/user/getMyVideo', params).then(res => {
        this.loading = false
        if (res.status === 0) {
          this.videos = res.data.list
          if (this.videos.length === 0) {
            this.hasData = true
          }
        }
      })
    }
  },
  created () {
    this.getMyVideos()
  }
}
</script>

<style lang="scss" scoped>
.video-box {
  height: 100%;
  margin-top: 1rem;
}
</style>
