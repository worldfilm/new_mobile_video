<template>
  <div class="VideoList">
    <div class="item" @click="toDetail(videoItem.id)" >
      <div class="img">
        <img v-lazy="videoItem.thumb_img_url" :key="videoItem.thumb_img_url">
        <div class="time">
          {{videoItem.created_at}}
        </div>
        <span class="iconfont icon-shoucang like" :class="{liked: collected}" @click="collect(videoItem.id)" v-if="showCollect"></span>
      </div>
      <p class="title">{{videoItem.title}}</p>
      <!-- <div class="desc clearfix">
        <div class="fl">{{item.views}}次观看</div>
        <div class="fr">{{item.like}}人喜欢</div>
      </div> -->
    </div>
    <!-- 插入广告 -->
    <a v-if='ad' :href="totalList.url" target="view_window">
      <div class="row1" >
        <img class="rowImg" :src="totalList.img_url"/>
        <!-- <span class="rowTitle">{{totalList.title}}</span> -->
      </div>
    </a>
    <!-- 插入广告 -->
  </div>
</template>

<script>
export default {
  props: {
    videoItem: {
      type: Object,
      default () {
        return {}
      }
    },
    test:{
      type: Object,
    },
    showCollect: {
      type: Boolean,
      default: true
    },
    isRow: {
      type: Boolean,
      default: true
    }
  },
  data () {
    return {
      isCollected: false,
      defaultImg:"this.src='/default1.jpg'",
      ad:false,
      totalList:[]
    }
  },
  computed: {
    collected () {
      return this.isCollected
    }
  },
  methods: {
    add(){
      let videoItem=this.videoItem
      // console.log(videoItem.xxx)
      for(var i in videoItem){
        if(videoItem.xxx){
          this.ad=true
        }
      }
    },
    //视频列表里插入广告
    getWebVideoList(){
      this.$http.get('/api/advert/list',{cate_code:'WebVideoList'}).then(res => {
        if (res.status === 0) {
          this.WebVideoList = res.data
        }
      })
    },
    collect (id) {
      if (!this.isCollected) {
        this.$collect(id, res => {
          if (res.status === 0) {
            this.isCollected = !this.isCollected
          }
        })
      } else {
        this.$cancelCollect(id, res => {
          if (res.status === 0) {
            this.isCollected = !this.isCollected
          }
        })
      }
    },
    toDetail (id) {
      this.$router.push({path: `/videoPlay/${id}`})
    }
  },
  created () {
    this.add()
    this.isCollected = this.videoItem.isCollects
    // debugger
    this.totalList = JSON.parse(window.sessionStorage.getItem('hotListData'))
  }
}
</script>

<style lang="scss" scoped>
// .rowTitle{
//     width: 100%;
//     font-size: 0.3rem;
//     font-weight: 600;
//     position: relative;
//     top: 0.1rem;
// }
.VideoList {
  margin-bottom: 0.2rem;
  display: inline;
  .item {
    display: inline-block;
    width: 48.5%;
    margin: 0.04rem;
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
    // width: 100%;
    display: block;
  }
  //
  // .clearfix:before,
  // .clearfix:after {
  //   display: table;
  //   content: "";
  // }
  //
  // .clearfix:after {
  //   clear: both;
  // }
}
</style>
