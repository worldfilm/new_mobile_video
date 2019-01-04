<template>
  <div class='Footer'>
    <div class="AppFooter" v-for="(item, index) in AppFooter" :key="index">
      <router-link :to="item.url" target="_blank">
        <img :src="item.img_url"/>
      </router-link>
    </div>
    <div class="list">
      <router-link v-for="item in navlist" :to='item.to'>
        <i :class="item.class"></i>
        <span v-text="item.name"></span>
      </router-link>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      selected: '/',
      AppFooter: [],
      navlist:[{name:'AV',class:'iconfont icon-shipin1',to:'/AV'},
               {name:'视频',class:'iconfont icon-shipin',to:'/Video'},
               {name:'分类',class:'iconfont icon-tag',to:'/Tag'},
               {name:'收藏',class:'iconfont icon-shoucang',to:'/Collect'},
               {name:'我的',class:'iconfont icon-wode',to:'/User'}]
    }
  },
  watch: {
    selected(newVal) {
      this.$router.push(newVal)
    }
  },
  created() {
    this.getbottomAdvertisingSectionList()
  },
  methods: {
    //底部插入广告
    getbottomAdvertisingSectionList() {
      this.$http.get('/api/advert/list', {
        cate_code: 'AppFooter'
      }).then(res => {
        if (res.status === 0) {
          this.AppFooter = res.data
        }
      })
    },
  }
}
</script>

<style lang="scss" scoped>
.Footer {
    z-index: 100;
    .AppFooter {
        position: fixed;
        bottom: 1rem;
        width: 100%;
        img {
            height: 1rem;
            width: 100%;
        }
    }
    .list {
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
                font-size: 0.25rem;
            }
            &.active {
                color: #FE7A94;
            }
            i{
              font-size: 0.4rem;
            }
        }
    }

}
</style>
