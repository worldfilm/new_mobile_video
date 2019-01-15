<template>
  <div class="VideoList" >
    <ul>
      <li v-for="item in list" class="item" @click="toDetail(list.id)">
          <div class="img">
            <img :src="list.thumb_img_url" :key="list.thumb_img_url">
            <div class="time">
              {{list.created_at}}
            </div>
            <span class="iconfont icon-shoucang like"  @click="collect(list.id)" ></span>
          </div>
          <p class="title">{{list.title}}</p>
        <!-- 插入广告 -->
        <!-- <a v-if='ad' :href="ADList.url" target="view_window">
          <div class="row1" >
            <img class="rowImg" :src="ADList.img_url"/>
          </div>
        </a> -->
        <!-- 插入广告 -->
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data () {
    return {
      isCollected: false,
      // defaultImg:"this.src='/default1.jpg'",
      ad:false,
      param: {
        categoryid: 2,
        page: 1,
        page_size: 40
      },
      list:[],
      ADList:[],
    }
  },
  methods: {
    // 获取视频列表
    getVideoList(data) {
      let param = this.param
      this.$http.get('/mapi/category/categorydetail', param).then(res => {
        if (res.status === 0) {
          // this.list=res.data.new
          let i=0,list=res.data.new
          // for(i in list){
          //     if(i > 1 && i % 6 == 0){
          //         list[i-1].ad = true
          //     }
          // }
          this.list=list
          console.log(list)
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
    toDetail(id){
      console.log(id)
    },
    collect(){},
  },
  mounted(){

    // console.log(this.ADList)
    // console.log(this.list)
  },
  created () {
    this.getVideoList()
    this.getAD()
    // debugger
  }
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
    float: left;
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
