<template>
  <div class="Tab" :class="{transition: !show}">
    <ul>
      <li v-for='(item,index) in list' v-text='item.name' :class="{active:index == num}" @click='TabSelect(index)'></li>
    </ul>
  </div>
</template>

<script>
import Hub from '@/components/Hub.vue'
export default {
  data () {
    return {
      list:[{name:'最新'},{name:'最热'}],
      selected: null,
      subSelect: 'new',
      dropDown: false,
      dropUp: false,
      show: true,
      num:0,
    }
  },
  methods: {
     TabSelect(idx){
       this.num=idx
       Hub.$emit('Tab', idx)
     },
  },
  mounted () {
  }
}
</script>

<style lang="scss" scoped>
.Tab {
  position: relative;
  top: 0.9rem;
  left: 0;
  width: 100%;
  height: 0.8rem;
  background: #fff;
  z-index: 10;
  ul{
    top: 1rem;
    left: 50%;
    width: 5rem;
    margin: 0 auto;
    background: #fff;
    border-radius: 0.33333rem;
    border: 0.02rem solid #fe7a94;
    font-size:0;
    li{
      width: 50%;
      font-size:0.3rem;
      display: inline-block;
      text-align: center;
      height: 0.8rem;
      line-height: 0.8rem;

    }
    .active {
      color: #fff;
      background: #fe7a94;
      border: 0px solid #fe7a94;
    }
  }
}
</style>
