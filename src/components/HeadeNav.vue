<template>
  <div class="HeadeNav" v-if="ShowHeadeNav">
    <ul>
      <li @click="NavClick(item.id,index)" v-for="(item,index) in list" v-text="item.name" :id="item.id" :style="{width:widthData}" :class="{active:index==num}"></li>
    </ul>
  </div>
</template>
<script>
import Hub from '@/components/Hub.vue'
export default {
  props: {
  },
  data() {
    return {
      ShowHeadeNav:true,
      num:0,
      widthData:0,
      list:[{id: 2, name: "黄金正片", thumb: "http://img.qxlsjw.com/appdev/fengmian/huangjinzhengpian.jpg"},
            {id: 3, name: "欧美专区", thumb: "http://img.qxlsjw.com/appdev/fengmian/oumei.jpg"},
            {id: 6, name: "日本无码", thumb: "http://img.qxlsjw.com/appdev/fengmian/boduoyejieyi.jpg"},
            {id: 13, name: "卡通动漫", thumb: "http://img.qxlsjw.com/appdev/fengmian/dongman.jpg"},
            {id: 16, name: "中文字幕", thumb: null}],
      list0:[{id: 2, name: "黄金正片", thumb: "http://img.qxlsjw.com/appdev/fengmian/huangjinzhengpian.jpg"},
            {id: 3, name: "欧美专区", thumb: "http://img.qxlsjw.com/appdev/fengmian/oumei.jpg"},
            {id: 6, name: "日本无码", thumb: "http://img.qxlsjw.com/appdev/fengmian/boduoyejieyi.jpg"},
            {id: 13, name: "卡通动漫", thumb: "http://img.qxlsjw.com/appdev/fengmian/dongman.jpg"},
            {id: 16, name: "中文字幕", thumb: null}],
      list1:[{id: 6, name: "最新更新"},
            {id: 13, name: "最热视频"},
            {id: 16, name: "自拍偷拍"}],
      list2:[{name: '女优列表',id: 0},
            {name: 'AV分类',id: 1},
            {name: '视频分类',id: 2}],
      list3:[{name: '女优',id: 0},
            {name: 'AV',id: 1},
            {name: '视频',id: 2}],
    }
  },
  methods: {
    NavClick(id,index){
      Hub.$emit('sendListId', id);
      this.num=index
    }
  },
  mounted(){
    Hub.$on('sendingIdx', data => {
      data==0?(this.list=this.list0):[]
      data==1?(this.list=this.list1):[]
      data==2?(this.list=this.list2):[]
      data==3?(this.list=this.list3):[]
      this.widthData=100/this.list.length+"%"
    });
    Hub.$on('ShowHeadeNav', data => {
       this.ShowHeadeNav=data
       if(data==false){
         Hub.$emit('marginData', 0);
       }else {
         Hub.$emit('marginData', '0.8rem');
       }
    });
  },
  created() {
    this.widthData=100/this.list.length+"%"
  }
}
</script>

<style lang="scss" scoped>
.HeadeNav {
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
    ul{
      width:100%;
      font-size: 0;
      li{
        flex-grow: 1;
        font-size: 0.25rem;
        line-height: 0.8rem;
        text-align: center;
        font-weight: 600;
            display: inline-block;
      }
    }
    .active {
        color: #fe7a94;
    }
}
</style>
