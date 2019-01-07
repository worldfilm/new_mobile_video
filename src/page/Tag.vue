<template>
  <div class="Tag">
    <v-tab :is-scroll="false" :tabs="tabs" @clickHandle="clickHandle">
      <v-actors v-if="componentId===0"></v-actors>
      <av-tag v-if="componentId===1"></av-tag>
      <videos-tag v-if="componentId===2"></videos-tag>
    </v-tab>
  </div>
</template>

<script>
import vTab from '@/components/tabs.vue'
import vActors from './Tag/actors.vue'
import avTag from './Tag/avTag.vue'
import videosTag from './Tag/videosTag.vue'
const tabs = [
  {
    label: '女优列表',
    id: 0
  },
  {
    label: 'AV分类',
    id: 1
  },
  {
    label: '视频分类',
    id: 2
  }
]

export default {
  components: {
    vTab,
    avTag,
    vActors,
    videosTag
  },
  data () {
    return {
      componentId: 0,
      tabs,
      videos: [],
      loading: false
    }
  },
  methods: {
    clickHandle (id) {
      this.componentId = id
    }
  }
}
</script>

<style scoped>
.Tag{
  /* width: 8rem; */
/* height: 20rem; */
}
</style>
