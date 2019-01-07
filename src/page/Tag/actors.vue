<template>
  <div class="actors">
    <div class="swiper-container">
    <div class="swiper-wrapper">
      <div class="swiper-slide" >
        <button class="item"  style="width:2rem;">按字母查询</button>
        <button  v-for="(item,index) in charts" :key="index" class='item' :class="{active:Select===index}"  @click="getchartlist(item,index)"  >
          {{item}}
        </button>
      </div>
    </div>
  </div>
    <div class="actors-container" v-loading="loading">
      <div v-for="(item, index) in actors" class="act-item" :key="index">
        <actor-item :actor-info="item" :can-collect="true"></actor-item>
      </div>
    </div>
  </div>
</template>

<script>
import actorItem from '@/components/actorItem.vue'
import Swiper from 'swiper';
import 'swiper/dist/css/swiper.min.css';
export default {
  components: {
    actorItem
  },
  data () {
    return {
      cups: ['A', 'B', 'C', 'D', 'E', 'F', 'G以上'],
      charts: ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z'],
      Select: 0,
      // cupSelected: '',
      actors: [],
      loading: false,
      param:{}
    }
  },
  methods: {
    getActors () {
      this.loading = true
      this.$http.get('/mapi/category/actorlist').then(res => {
        this.loading = false
        if (res.status === 0) {
          let list =res.data
          for(var i in list){
              list[i].local_head_url=this.$MyConfig.baseURL+list[i].local_head_url
          }
          this.actors = list
        }
      })
    },
    getchartlist(item,index){
      this.Select=index
      this.loading = true
      let param=this.param
      this.param.name_first_char=item
      this.$http.get('/mapi/category/actorlist',param).then(res => {
        this.loading = false
        if (res.status === 0) {
          let list =res.data
          for(var i in list){
              list[i].local_head_url=this.$MyConfig.baseURL+list[i].local_head_url
          }
          this.actors = list
        }
      })
    }
  },
  mounted(){
       var mySwiper2 = new Swiper('.swiper-container', {
         scrollbar: '.swiper-scrollbar',
         direction: 'horizontal',
         slidesPerView: 'auto',
         freeMode: true
       })
     },
  created () {
    this.getActors()
    this.getchartlist('A',0)
  }
}
</script>

<style lang="scss" scoped>
.item {
  height: .7rem;
  line-height: .7rem;
  width: 1rem;
  padding: 0 0.3rem;
  margin: 0.1rem;
  text-align:center;
  background-color: rgba(254, 122, 148, 0.1);
display: inline-block;
padding: 0 0.13333rem;
font-size: 0.25rem;
color: #FE7A94;
border-radius: 0.05333rem;
-webkit-box-sizing: border-box;
box-sizing: border-box;
border: 0.01333rem solid rgba(254, 122, 148, 0.2);
white-space: nowrap;
}
.active {
  background: #FE7A94;
  color: #fff;
}
.actors-container {
  margin-top: 0.2rem;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  .act-item {
    width: 25%;
    margin-bottom: 0.2rem;
    text-align: center;
  }
}
.swiper-container-android .swiper-slide, .swiper-wrapper{
      width: max-content;
}
.el-tag--danger{
  margin:0rem 0.1rem;
  height: .7rem;
  line-height: .7rem;
  width: .7rem;
  text-align: center;
}
</style>
