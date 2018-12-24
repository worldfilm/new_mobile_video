<template>
  <div class="Tag">
    <Navheader/>
    <div class="swiper-container">
      <div class="swiper-wrapper">
        <div class="swiper-slide">
          <ActorsTab/>
        </div>
      </div>
    </div>

    <!-- <v-tab :is-scroll="false" :tabs="tabs" @clickHandle="clickHandle">
      <v-actors v-if="componentId===0"></v-actors>
      <av-tag v-if="componentId===1"></av-tag>
      <videos-tag v-if="componentId===2"></videos-tag>
    </v-tab> -->
    <Footer/>
  </div>
</template>
<script>
import Hub from '@/components/Hub.vue'
import Navheader from '@/components/Navheader.vue'
import Footer from '@/components/Footer.vue'
import ActorsTab from '@/components/ActorsTab.vue'
export default {
  components: {
    Navheader,
    Footer,ActorsTab
  },
  data () {
    return {
      // componentId: 0,
      // tabs,
      // videos: [],
      // loading: false
      titlelist:[{name: '女优列表',id: 0},{name: 'AV分类',id: 1},{name: '视频分类',id: 2}],
    }
  },
  methods: {
    // clickHandle (id) {
    //   this.componentId = id
    // }
    // 获取分类
    getTabList() {
      this.$http.get('/mapi/category/getvediolist').then(res => {
        if (res.status === 0) {
           Hub.$emit('titlelist',this.titlelist)
        }
      })
    },
  },
  created() {
    this.getTabList()
  }
}
</script>
<style lang="scss" scoped>
.Tag {
      margin-top: 1rem;
    .swiper-container {
        z-index: 0;

    }
}
</style>
