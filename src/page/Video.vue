<template>
  <div class="Video">
    <div class="VideoList" v-if="activeIndex==1">
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
      heightData:'',
      ScrollChange:'',
      param: {
        categoryid: 9,
        page: 1,
        page_size: 10
      },
      resStr: 'new',
      loading: false,
      list:[],
      ADList:[],
      ad:false,
      num:0,
      activeIndex:'',
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
      // let id=this.param.categoryid
      // console.log('id='+id)
      // if(data==undefined){
      //   this.param.categoryid=9
      // }else{
      //   this.param.categoryid=data
      //   this.param.page_size=6
      // }
      if(this.param.categoryid!=undefined&&this.activeIndex==1){
        this.list=[];
        let num=this.num
        num=num+1
        this.num=num
        let categoryid=this.param.categoryid
        let page_size=this.param.page_size
        let page=this.param.page
        this.num=this.num+1
        page_size=this.num/2*6
        console.log('Video请求='+this.num/2)
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
          }
        })
      }
    },
    // 视频列表Video插入广告
    getAD(){
      this.$http.get('/api/advert/list',{cate_code:'AppVideoListVideo'}).then(res => {
        if (res.status === 0) {
          this.ADList=res.data[0]
        }
      })
    },
    handleScrollx(){
        // 页面滚动距顶部距离
        var scrollTop = window.pageYOffset || document.documentElement.scrollTop ||document.body.scrollTop
        var scroll = scrollTop - this.i;
        this.i = scrollTop;
        let ScrollChange=document.documentElement.scrollTop //滚动条高度
        let ClientHeight=document.documentElement.clientHeight//浏览器高度
        let OffsetHeight=document.documentElement.offsetHeight//网页高度
        let numchange=OffsetHeight-ClientHeight
        let num=this.num
        if(ScrollChange==numchange){
          let categoryid=this.param.categoryid
            this.getVideoList(categoryid)
        }
    },
  },
  mounted(){
    window.addEventListener('scroll', this.handleScrollx, true);
  },
  created () {
    this.getCategory()// 获取分类
    this.widthData=document.documentElement.clientWidth
    this.heightData=document.documentElement.clientHeight
    this.ScrollChange=document.documentElement.scrollTop
    Hub.$on('sendListId', data => {
      this.param.page_size=0
        this.getVideoList(data)
    });
    Hub.$on('secondQuery', data => {
      this.param.categoryid=data
        this.getVideoList(data)
    });
    Hub.$on('activeIndex', data => {
        this.activeIndex=data
        if(data==1){
          this.this.param.page_size=6
        }
    });
    this.getVideoList(9)// 获取视频列表
    this.getAD()// 视频列表Video插入广告
  },
}
</script>

<style lang="scss" scoped>
.rowImg{
  height: 2.5rem;
  width: 100%;
}
.Video{
  overflow: hidden;
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
}
</style>
