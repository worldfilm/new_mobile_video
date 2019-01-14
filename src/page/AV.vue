<template>
  <div class="AV" :style="{width:widthData+'px'}">
      <!-- <v-tab :isSubTab="true" @refresh="param.page++" :tabs="tabs" @clickHandle="clickHandle"> -->
        <!-- <div style="height: 100%" class="clearfix" @refresh="param.page++" v-loading="loading"> -->
          <VideoList/>
        <!-- </div> -->
      <!-- </v-tab> -->
  </div>
</template>

<script>
import vTab from '@/components/tabs.vue'
import VideoList from '@/components/VideoList.vue'
import Footer from '@/components/Footer.vue'
import Hub from '@/components/Hub.vue'
export default {
  components: {
    vTab,VideoList,Footer
  },
  data () {
    return {
      widthData:'',
      param: {
        categoryid: 2,
        page: 1,
        page_size: 40
      },
      resStr: 'new',
      loading: false,
      list:[],
      ADList:[],
    }
  },
  methods: {
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
          this.param.categoryid = 9
        }
      })
    },

    // 获取视频列表
    getVideoList(data) {
      // if(data!=undefined){
        this.param.categoryid=data
        // console.log(this.param.categoryid)
      // }
      let param = this.param
      this.$http.get('/mapi/category/categorydetail', param).then(res => {
        if (res.status === 0) {
          this.list=res.data.new
          let i=0,list=this.list
          for(i in list){
              if(i > 1 && i % 6 == 0){
                // console.log(i-1)
                  list[i-1].ad = true
              }
          }
          this.list=list
        }
      })
    },
    // 视频列表Video插入广告
    getAD(){
      this.$http.get('/api/advert/list',{cate_code:'AppVideoListVideo'}).then(res => {
        if (res.status === 0) {
          this.ADList=res.data
        }
      })
    },
    // 视频列表Video插入广告
    // InsertAD(){
    //   // console.log(this.ADList)
    //   // console.log(this.list)
    //   let i=0,list=this.list,ADList=this.ADList,j=0
    //   for(i in list){
    //       if(i > 1 && i % 6 == 0){
    //         // console.log(i-1)
    //           list[i-1].ad = true
    //       }
    //   }
    //   this.list=list
    // },
  },
  beforeUpdate(){

    // console.log(this.ADList)
    // console.log(this.list)
  },
  created () {
    this.getCategory()// 获取分类

    this.widthData=document.documentElement.clientWidth
    this.heightData=document.documentElement.clientHeight
    Hub.$on('sendListId', data => {
        this.getVideoList(data)
        // console.log(data)
    });
    this.getVideoList()// 获取视频列表
    this.getAD()// 视频列表Video插入广告
  }
}
</script>

<style scoped>
.AV{
  /* width: 8rem; */
/* height: 20rem; */
}
</style>
