<template>
  <div class="header-bar">
    <span class="left-btn iconfont icon-jiantou" @click="goBack"></span>
    <div class="title">{{title}}</div>
    <div class="right-btn">
      <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    titler: {
      type: String
    }
  },
  data () {
    return {}
  },
  computed: {
    title () {
      return this.$route.meta.title || this.titler
    }
  },
  methods: {
    goBack () {
      window.history.length > 1
        ? this.$router.go(-1)
        : this.$router.push('/')
    }
  }
}
</script>

<style lang="scss" scoped>
.header-bar{
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 0.8rem;
  line-height: 0.8rem;
  text-align: center;
  font-size: 0.3rem;
  box-shadow: -1px 0px 1px #ccc;
  background: #fff;
  z-index: 1000;
  .iconfont.icon-jiantou {
    font-size: 0.3rem;
    transform: rotate(90deg);
  }
  .left-btn,.right-btn {
    position: absolute;
    top: 0;
  }
  .left-btn {
    left: 0;
    padding-left: 0.1rem;
  }
  .right-btn {
    right: 0;
    padding-right: 0.1rem;
  }
}
</style>
