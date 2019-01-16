<template>
  <div class="Tag" :style="{width:widthData+'px'}">
    <div v-if="activeIndex==2">
      <ActorSilde/>
    </div>
    <!-- <v-tab :is-scroll="false" :tabs="tabs" @clickHandle="clickHandle">
      <v-actors v-if="componentId===0"></v-actors>
      <av-tag v-if="componentId===1"></av-tag>
      <videos-tag v-if="componentId===2"></videos-tag>
    </v-tab> -->
  </div>
</template>

<script>
import vTab from '@/components/tabs.vue'
import ActorSilde from '@/components/ActorSilde.vue'
import avTag from './Tag/avTag.vue'
import videosTag from './Tag/videosTag.vue'
import Hub from '@/components/Hub.vue'
export default {
  components: {
    vTab,
    avTag,
    ActorSilde,
    videosTag
  },
  data () {
    return {
      componentId: 0,
      videos: [],
      loading: false,
      widthData:'',
      activeIndex:'',
    }
  },
  methods: {
    clickHandle (id) {
      this.componentId = id
    }
  },
  created () {
    this.widthData=document.documentElement.clientWidth
    Hub.$on('activeIndex', data => {
        this.activeIndex=data
    });
  }
}
</script>

<style scoped>
.Tag{
  overflow:hidden;
  /* width: 8rem; */
/* height: 20rem; */
}
</style>
