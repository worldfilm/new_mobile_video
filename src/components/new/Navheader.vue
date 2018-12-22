<template>
  <div class="Navheader">
    <ul>
      <li v-for='(item,index) in list' :id='item.id' :style="{width: widthData}" @click='getlist(item.id,index)'  :class="{active:index == num}">
        <span  v-text='item.name'></span>
      </li>
    </ul>
  </div>
</template>
<script>
import Hub from '@/components/Hub.vue'
export default {
  data() {
    return {
      list: null,
      widthData:null,
      num:0,
      id:2,
    }
  },
  methods: {
    // 发送列表id
    getlist(id,idx){
      this.num=idx
      this.id=id
      Hub.$emit('Navheader', this.id)
    },
    tabClick(id) {
      if (this.selected === id) return
      this.subSelect = 'new'
      this.selected = id
      // this.$emit('clickHandle', id, this.subSelect)
    },
    // 获取导航列表
    getCategory() {
      this.$http.get('/mapi/category/getvediolist').then(res => {
        if (res.status === 0) {
          this.list = res.data
          this.widthData=100/this.list.length+'%'
        }
      })
    },
  },
  mounted() {
    this.getCategory()
  },
  created() {
    // console.log(this.tlist)
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
