<template>
  <div class="container">
    <v-header :seller="seller"></v-header>
    <div class="tab">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评价</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <router-view :seller="seller"></router-view>
  </div>
</template>

<script>
  import header from '@/components/header/header'

  const ERR_OK = 0

  export default {
    components: {
      'v-header': header
    },
    data () {
      return {
        seller: {}
      }
    },
    created () {
      this.$http.get('api/seller').then(res => {
        res = res.body
        if (res.errno === ERR_OK) {
          this.seller = res.data
        }
      })
    }
  }
</script>

<style lang="scss" rel="stylesheet/scss">
  @import "common/sass/app";
  .tab {
    display: flex;
    height: 40px;
    line-height: 40px;
    border-bottom: 1px solid rgba(7, 17, 27, .1);
    &-item {
      flex: 1;
      text-align: center;
      font-size: 14px;
      a {
        display: block;
        color: rgb(77, 85, 93);

        &.router-link-active {
          color: rgb(240, 20, 20);
        }
      }
    }
  }
</style>
