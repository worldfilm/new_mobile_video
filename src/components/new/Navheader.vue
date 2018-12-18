<template>
  <div class="Navheader">
    <div class="tabHeader shadow">
      <a href="javascript:;"  >
      
      </a>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      selected: null,
      subSelect: 'new',
      dropDown: false,
      dropUp: false,
      show: true
    }
  },
  watch: {
  },
  methods: {
    tabClick (id) {
      if (this.selected === id) return
      this.subSelect = 'new'
      this.selected = id
      // this.$emit('clickHandle', id, this.subSelect)
    },
  },
  mounted () {
  }
}
</script>

<style lang="scss" scoped>
.shadow {
  box-shadow: 0px 2px 8px #fe7a94;
}
.tabHeader {
  position: fixed;
  top: 0;
  left: 0;
  display: flex;
  display: -webkit-flex;
  width: 100%;
  height: 0.8rem;
  background: #fff;
  z-index: 100;
  transition: all .3s;
  a {
    flex-grow: 1;
    font-size: 0.25rem;
    line-height: 0.8rem;
    text-align: center;
    font-weight: 600;
    &.active {
      color: #fe7a94;
    }
  }
}
.content-header {
    top: 1rem;
    left: 50%;
    transform: translate(-50%);
    width: 70%;
    margin: 0 auto;
    background: #fff;
    border-radius: 25px;
    a {
      width: 50%;
      border-radius: 0 25px 25px 0;
      border: 2px solid #fe7a94;
      margin-right: -1px;
      &:first-child {
        border-radius: 25px 0 0 25px;
        margin-left: -1px;
      }
      &.active {
        color: #fff;
        background: #fe7a94;
      }
    }
  }
.tabContainer {
  position: absolute;
  top: 0.8rem;
  left: 0;
  width: 100%;
  height: calc(100% - 1rem);
  padding: 0rem;
  overflow: hidden;
}
._tabContainer {
  top: 1.7rem;
  height: calc(100% - 2rem);
}
.wrapper {
  padding-bottom: 0.8rem;
  position: relative;
  .loading {
    position: absolute;
    bottom: 0;
    left: 0;
    width: 100%;
    height: 0.8rem;
    line-height: 0.8rem;
    text-align: center;
    .el-icon-loading {
      font-size: 0.6rem;
    }
  }
}
.transition {
  height: 0 !important;
  opacity: 0;
  transition: all .3s;
}
._height {
  top: 0.8rem;
  height: calc(100% - 1.2rem);
}
</style>
