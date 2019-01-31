<template>
  <div class="AV" :style="{width:widthData+'px'}"  @touchend='touchEnd'>
     <div class="VideoList" v-if="activeIndex==0">
      <ul>
        <li v-for="item in list" class="item" @click="toDetail(list.id)">
            <div class="img">
              <img :src="item.thumb_img_url" :key="item.thumb_img_url">
              <div class="time">
                {{list.created_at}}
              </div>
              <span class="iconfont icon-shoucang like"  @click="collect(list.id)" ></span>
            </div>
            <p class="title">{{item.title}}</p>
          <!-- 插入广告 -->
           <a class="adlink" v-if="item.ad"  :href="item.ad.url" target="view_window">
              <img class="adimg" :src="item.ad.img_url"/>
          </a>
          <!-- 插入广告 -->
         </li>
      </ul>
    </div>
  </div>
</template>
<script>
import tabs from '@/components/tabs.vue'
import VideoList from '@/components/VideoList.vue'
import Footer from '@/components/Footer.vue'
import Hub from '@/components/Hub.vue'
  import Scroll from '@/components/scroll.vue'
export default {
  components: {
    tabs,VideoList,Footer,Scroll
  },
  data () {
    return {
      widthData:'',
      heightData:'',
      ScrollChange:'',
      resStr: 'new',
      loading: false,
      list:[],
      ADList:[],
      ad:false,
      num:8,
      activeIndex:0,
      page_size:8,
      categoryid:2,
      page: 1,
      items: [],
      startY: 0,
    }
  },
  methods: {
    touchEnd (e) {
      let scrollIsToTop = document.documentElement.scrollTop || window.pageYOffset || document.body.scrollTop // safari 获取scrollTop用window.pageYOffset
      let OffsetHeight=document.documentElement.offsetHeight
      let clientHeight=document.documentElement.clientHeight//浏览器高度
      let numberN=scrollIsToTop+clientHeight+56
      if(numberN==OffsetHeight){
        let num=this.num+8
        this.num=num
        this.getVideoList(this.categoryid,this.num)
      }
    },
    clickItem() {
      this.$router.back()
    },
    // 视频列表Video插入广告
    getAD(){
      this.$http.get('/api/advert/list',{cate_code:'AppVideoListAV'}).then(res => {
        if (res.status === 0) {
          if(res.data.length>0){
            this.ADList=res.data[0]
          }
        }
      })
    },
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
          // this.categoryid = 9
        }
      })
    },
    // 获取视频列表
    getVideoList(categoryid,num) {
      console.log(' num='+num)
      if(this.categoryid!=undefined&&this.activeIndex==0){
        this.page_size=num
        let page=this.page
        let page_size=this.page_size
        // page_size=(num+1)*page_size
        this.$http.get('/mapi/category/categorydetail?categoryid='+categoryid+'&page='+page+'&page_size='+page_size).then(res => {
          if (res.status === 0) {
            this.list=res.data.new
            let i=0,list=this.list
            for(i in list){
                if(i > 1 && i % 6 == 0){
                    list[i-1].ad = this.ADList
                }
            }
            this.list=list
            // console.log(this.list)
          }
        })
      }
    },

    handleScroll(){  // 页面滚动距顶部距离
      // let scrollIsToTop = document.documentElement.scrollTop || window.pageYOffset || document.body.scrollTop // safari 获取scrollTop用window.pageYOffset
      // let OffsetHeight=document.documentElement.offsetHeight
      // let clientHeight=document.documentElement.clientHeight//浏览器高度
      // let numberN=scrollIsToTop+clientHeight
      // // console.log('numberN='+numberN+'  OffsetHeight='+OffsetHeight)
      // if(numberN==OffsetHeight){
      //   // console.log('numberN='+numberN+'  OffsetHeight='+OffsetHeight)
      //   let num=this.num+8
      //   this.num=num
      //   let categoryid=this.categoryid
      //   this.getVideoList(categoryid,this.num)
      // }
    },
  },
  mounted(){
    let categoryid=2,num=8
    this.getVideoList(categoryid,num)// 获取视频列表
    Hub.$on('AVsendId', categoryid => {
        this.getVideoList(categoryid,num)
    });
  },
  updated(){
    // window.addEventListener('scroll', this.handleScroll, true);
  },
  created () {
    this.getAD()// 视频列表Video插入广告
    this.getCategory()// 获取分类
    let categoryid=this.categoryid,num=this.num
    this.widthData=document.documentElement.clientWidth
    this.heightData=document.documentElement.clientHeight
    this.ScrollChange=document.documentElement.scrollTop
    Hub.$on('sendListId', data => {
      console.log('sendListId='+data)
        this.categoryid=data
        this.num=8
        num=this.num
        this.list=[]
        this.getVideoList(this.categoryid,num)
    });
  },
}
</script>

<style lang="scss" scoped>
.rowImg{
  height: 2.5rem;
  width: 100%;
}
.VideoList {
  margin-bottom: 0.2rem;
  display: inline;
  .item {
    display: inline-block;
    width: 48.5%;
    margin: 0.04rem;
    float:left;
    .img {
      position: relative;
      text-align: center;
      img {
        width: 3.7rem;
        height:2.5rem;
        display: block;
      }
      .time {
        position: absolute;
        bottom: 0;
        left: 0;
        width: 100%;
        height: 0.5rem;
        padding-left: 0.1rem;
        color: #fff;
        line-height: 0.5rem;
        text-align: left;
        background: rgba($color: #000000, $alpha: 0.3);
      }
      .like {
        position: absolute;
        top: 0.2rem;
        right: 0.2rem;
        padding: 0.08rem;
        font-size: 0.5rem;
        color: pink;
        background: rgba($color: #000, $alpha: 0.5);
        border-radius: 50%;
      }
      .liked {
        background: rgba($color: #ec4e63, $alpha: 0.5);
      }
    }
    .title {
      font-size: 0.2rem;
      height: 0.35rem;
      line-height: 0.35rem;
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
    }
    .desc {
      font-size: 0.3rem;
      padding: 0 0.2rem;
      color: #ccc;
    }
  }
  .time {
    font-size: 13px;
    color: #999;
  }
  .image {
    display: block;
  }
}
.adlink{
  width: 7.4rem;
  height: 2.5rem;
    position: relative;
    left: -3.7rem;
    display: block;
    .adimg{
      height: 2.5rem;
      width: 7.4rem;
    }
}
</style>
