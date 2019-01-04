<template>
  <div class="Navheader">
    <ul>
      <li v-for='(item,index) in titlelist' :id='item.id' :style="{width: widthData}" @click='getlist(item.id,index)'  :class="{active:index == num}">
        <span  v-text='item.name'></span>
      </li>
    </ul>
  </div>

</template>
<script>
import Hub from '@/components/Hub.vue'
export default {
  props:['title'],
  data() {
    return {
      list: null,
      widthData:null,
      num:0,
      id:2,
      titlelist:[],
      AVlist:[{id: 2, name: "黄金正片", thumb: "http://img.qxlsjw.com/appdev/fengmian/huangjinzhengpian.jpg"},
               {id: 3, name: "欧美专区", thumb: "http://img.qxlsjw.com/appdev/fengmian/oumei.jpg"},
               {id: 6, name: "日本无码", thumb: "http://img.qxlsjw.com/appdev/fengmian/boduoyejieyi.jpg"},
               {id: 13, name: "卡通动漫", thumb: "http://img.qxlsjw.com/appdev/fengmian/dongman.jpg"},
               {id: 16, name: "中文字幕", thumb: null}],
     VideoList:[{name: '最新更新',id: 7},{name: '最热视频',id: 9},{name: '自拍偷拍',id: 24}],
     TagList:[{name: '女优列表',id: 0},{name: 'AV分类',id: 1},{name: '视频分类',id: 2}],
     CollectList:[{name: '女优',id: '0'},{name: 'AV',id: '1'},{name: '视频',id: '2'}]
    }
  },
  methods: {
    // 发送列表id
    getlist(id,idx){
      this.num=idx
      this.id=id
      Hub.$emit('Navheader',id)
      console.log(id)

    },
    // tabClick(id) {
    //   if (this.selected === id) return
    //   this.subSelect = 'new'
    //   this.selected = id
    // },
    // 获取导航列表
    getNavList() {
      this.$http.get('/mapi/category/getvediolist').then(res => {
        if (res.status === 0) {
          this.titlelist=this.AVlist
          this.widthData=100/this.AVlist.length+'%'
          // this.list = res.data
          // this.widthData=100/this.list.length+'%'
        }
      })
    },
    getList(){
      let title=this.title
          if(title=='AV'){
            // this.getNavList()
             this.titlelist=this.AVlist
             this.widthData=100/this.AVlist.length+'%'
          }
          if(title=='Video'){
            this.titlelist=this.VideoList
            this.widthData=100/this.VideoList.length+'%'
          }
          if(title=='Tag'){
            this.titlelist=this.TagList
            this.widthData=100/this.VideoList.length+'%'
          }
          if(title=='Collect'){
            this.titlelist=this.CollectList
            this.widthData=100/this.VideoList.length+'%'
          }
    },
  },
  mounted(){
    this.getList()
  },
  created() {
     // this.getList()
 }
}
</script>

<style lang="scss" scoped>
.Navheader {
    position: fixed;
    top: 0;
    left: 0;
    display: flex;
    display: -webkit-flex;
    width: 100%;
    height: 0.8rem;
    background: #fff;
    z-index: 100;
    transition: all 0.3s;
    box-shadow: 0 2px 8px #fe7a94;
    ul {
        width: 100%;
        li {
            display: inline-block;
            text-align: center;
            width:20%;
            span {
                font-size: 0.25rem;
                line-height: 0.8rem;
                display: block;
                font-weight: 600;
            }
        }
        .active {
            color: #fe7a94;
        }
    }
}
</style>
