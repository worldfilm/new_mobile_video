<template>
<div class="Footer">
    <div class="bottomAdvertisingSection" v-for="(item, index) in bottomAdvertisingSectionList" :key="index">
        <a :href="item.url"  target="_blank" >
          <img :src="item.img_url"/>
        </a>
    </div>
    <div class="footer">
        <router-link :to='item.link' v-for="(item,index) in list" :class="{isactive:index == num}">
            <span :class=" item.class" ></span>
            {{item.name}}
        </router-link>
    </div>
</div>
</template>

<script>
import Hub from '@/components/Hub.vue'
export default {
  data () {
    return {
      selected: '/',
      bottomAdvertisingSectionList:[],
      num:0,
      list:[
        {name:"AV",class:"iconfont icon-shipin1",link:"AV"},
        {name:"视频",class:"iconfont icon-shipin",link:"Video"},
        {name:"分类",class:"iconfont icon-tag",link:"Tag"},
        {name:"收藏",class:"iconfont icon-shoucang",link:"Collect"},
        {name:"我的",class:"iconfont icon-wode",link:"User"},
        ]
    }
  },
  created () {
    this.getbottomAdvertisingSectionList()
    Hub.$on('sendingIdx', data => {
      this.num=data
    });
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
    height: 1rem;
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
  a {
    display: flex;
    flex-direction: column;
    flex-grow: 1;
    font-size: 0.2rem;
    justify-content: center;
    align-items: center;
    span {
        font-size: 0.5rem;
    }
  }
  .isactive {
    color: #FE7A94;
  }
}
</style>
