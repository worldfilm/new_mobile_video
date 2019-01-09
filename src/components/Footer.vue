<template>
<div class="Footer" v-if="ShowFooter">
    <div class="bottomAdvertisingSection" v-for="(item, index) in bottomAdvertisingSectionList" :key="index">
        <a :href="item.url"  target="_blank" >
          <img :src="item.img_url"/>
        </a>
    </div>
    <div class="footer">
      <ul>
        <li v-for="(item,index) in list" :class="{isactive:index == num}">
            <i :class="item.class"></i>
            <span v-text="item.name"></span>
        </li>
      </ul>
    </div>
</div>
</template>

<script>
import Hub from '@/components/Hub.vue'
export default {
  data () {
    return {
      ShowFooter:true,
      selected: '/',
      bottomAdvertisingSectionList:[],
      num:0,
      list:[{name:"AV",class:"iconfont icon-shipin1",link:"AV"},
            {name:"视频",class:"iconfont icon-shipin",link:"Video"},
            {name:"分类",class:"iconfont icon-tag",link:"Tag"},
            {name:"收藏",class:"iconfont icon-shoucang",link:"Collect"},
            {name:"我的",class:"iconfont icon-wode",link:"User"}],
    }
  },

  methods:{
    //底部插入广告
    getbottomAdvertisingSectionList(){
      this.$http.get('/api/advert/list',{cate_code:'AppFooter'}).then(res => {
        if (res.status === 0) {
          this.bottomAdvertisingSectionList = res.data
        }
      })
    },
  },
  created () {
    this.getbottomAdvertisingSectionList()
    Hub.$on('sendingIdx', data => {
      this.num=data
    });
    Hub.$on('ShowFooter', data => {
      this.ShowFooter=data
    });
  }
}
</script>

<style lang="scss" scoped>
.bottomAdvertisingSection{
    position: fixed;
    bottom: 1rem;
    width: 100%;
}
.bottomAdvertisingSection img{
    height: 1.1rem;
    width: 100%;
}
.footer {
  display: flex;
  display: -webkit-flex;
  flex-wrap: nowrap;
  width: 100%;
  height: 1rem;
  background: #fff;
  box-shadow: 0 -1px 1px #ccc;
  position: fixed;
  bottom: 0;
  left: 0;
  ul{
      width: 100%;
    li{
      display: inline-block;
      width: 20%;
      font-size: 0.25rem;
      text-align: center;
      i{
        font-size: 0.45rem;
        display: block;
        height: 0.6rem;
        line-height: 0.6rem;
      }
      span{
        // font-size: 0.5rem;
      }
    }
    .isactive {
      color: #FE7A94;
    }
  }
}
</style>
