<template>
  <div class="header">
    <div class="content-wrap">
      <div class="avatar">
        <img :src="seller.avatar" width="64" height="64" alt="商家图片">
      </div>
      <div class="content">
        <div class="title">
          <span class="icon"></span>
          <span class="name">{{ seller.name }}</span>
        </div>
        <p class="desc">{{seller.description}}</p>
        <div class="supports">
          <div class="support">
            <i class="icon" :class="classMap[seller.supports[0].type]"></i>
            <span class="text">{{seller.supports[0].description}}</span>
          </div>
        </div>
      </div>
    </div>
    <div class="bulletin-wrap" @click="detailShow = true">
      <i class="icon"></i>
      <span class="text">{{seller.bulletin}}</span>
      <i class="fa fa-angle-right"></i>
    </div>
    <div class="supports-count" @click="detailShow = true">
      <span>{{seller.supports.length}}个</span>
      <i class="fa fa-angle-right"></i>
    </div>
    <div class="img">
      <img :src="seller.avatar" alt="">
    </div>
    <transition name="fade">
      <div v-show="detailShow" class="detail-wrap">
        <div class="content">
          <div class="main">
            <h1 class="title">{{seller.name}}</h1>
            <div class="star-wrap">
              <star :size="48" :score="seller.score"></star>
            </div>
            <section class="section">
              <div class="section-title">
                <span class="line"></span>
                <span class="text">优惠信息</span>
                <span class="line"></span>
              </div>
              <div class="section-content">
                <ul class="supports">
                  <li v-for="item in seller.supports">
                    <i class="icon" :class="classMap[item.type]"></i>
                    <span class="text">{{item.description}}</span>
                  </li>
                </ul>
              </div>
            </section>
            <section class="section">
              <div class="section-title">
                <span class="line"></span>
                <span class="text">商家公告</span>
                <span class="line"></span>
              </div>
              <div class="section-content">
                <p class="bulletin">{{seller.bulletin}}</p>
              </div>
            </section>
          </div>
        </div>
        <div class="close" @click="detailShow = false">
          <i class="fa fa-times"></i>
        </div>
      </div>
    </transition>
  </div>
</template>

<script type="text/ecmascript-6">
  import star from '@/components/star/star'
  export default {
    props: {
      seller: {
        type: Object
      }
    },
    created () {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
    },
    data () {
      return {
        classMap: [],
        detailShow: false
      }
    },
    components: {
      star
    }
  }
</script>

<style lang="scss" rel="stylesheet/scss">
  @import "../../common/sass/mixin";

  .header {
    position: relative;
    padding-bottom: 28px;
    background-color: rgba(7, 17, 27, .5);
    color: #fff;
    .content-wrap {
      display: flex;
      padding: 24px 24px 18px;
      color: #fff;
      .avatar {
        width: 64px;
        height: 64px;
        margin-right: 12px;
      }
      .content {
        flex: 1;
      }
      .title {
        margin-bottom: 8px;
        font-size: 16px;
        line-height: 18px;
        font-weight: 700;
        .icon {
          display: inline-block;
          width: 30px;
          height: 18px;
          @include bg-img('brand');
          background-size: 30px 18px;
          vertical-align: middle;
        }
      }
      .desc {
        margin-bottom: 10px;
        font-size: 12px;
      }
      .support {
        .icon {
          display: inline-block;
          width: 12px;
          height: 12px;
          margin-right: 4px;
          vertical-align: middle;
          background-size: 12px 12px;
          &.decrease {
            @include bg-img('decrease_1');
          }
          &.discount {
            @include bg-img('discount_1');
          }
          &.special {
            @include bg-img('special_1');
          }
          &.invoice {
            @include bg-img('invoice_1');
          }
          &.guarantee {
            @include bg-img('guarantee_1');
          }
        }
      }
    }
    .bulletin-wrap {
      position: absolute;
      bottom: 0;
      left: 0;
      right: 0;
      height: 28px;
      padding-left: 12px;
      padding-right: 27px;
      font-size: 10px;
      line-height: 28px;
      color: #fff;
      overflow: hidden;
      white-space: nowrap;
      text-overflow: ellipsis;
      background-color: rgba(7, 17, 27, .2);
      .icon {
        display: inline-block;
        width: 22px;
        height: 12px;
        vertical-align: middle;
        @include bg-img('bulletin');
        background-size: 22px 12px;
      }
      .fa {
        position: absolute;
        right: 12px;
        top: 50%;
        transform: translateY(-50%);
      }
    }
    .supports-count {
      display: block;
      position: absolute;
      right: 12px;
      bottom: 44px;
      padding-left: 8px;
      padding-right: 8px;
      font-size: 10px;
      line-height: 24px;
      border-radius: 10px;
      background: rgba(0, 0, 0, .2);
    }
    .img {
      position: absolute;
      top: 0;
      bottom: 0;
      left: 0;
      right: 0;
      z-index: -1;
      filter: blur(20px);
      overflow: hidden;
      img {
        width: 100%;
        height: 100%;
      }
    }
    .detail-wrap {
      position: fixed;
      top: 0;
      left: 0;
      z-index: 99;
      width: 100%;
      height: 100%;
      overflow: auto;
      transition: all .5s;
      -webkit-backdrop-filter: blur(10px);
      background-color: rgba(7, 17, 27, .8);
      -webkit-overflow-scrolling: touch;
      .star-wrap {
        margin-top: 24px;
        margin-bottom: 28px;
        text-align: center;
      }
      .supports {
        li {
          margin-bottom: 12px;
          font-size: 12px;
        }
        .icon {
          display: inline-block;
          width: 16px;
          height: 16px;
          margin-right: 6px;
          vertical-align: middle;
          background-size: 16px 16px;
          &.decrease {
            @include bg-img('decrease_2');
          }
          &.discount {
            @include bg-img('discount_2');
          }
          &.special {
            @include bg-img('special_2');
          }
          &.invoice {
            @include bg-img('invoice_2');
          }
          &.guarantee {
            @include bg-img('guarantee_2');
          }
        }
      }
      .content {
        min-height: 100%;
      }
      .main {
        padding-top: 64px;
        padding-bottom: 64px;
      }
      .close {
        position: relative;
        width: 32px;
        height: 32px;
        margin: -64px auto 0 auto;
        font-size: 32px;
      }
      .title {
        font-size: 16px;
        font-weight: 700;
        text-align: center;
      }
      .section {
        margin: 28px 36px;
        &-title {
          display: flex;
          .text {
            padding: 0 12px;
            font-size: 14px;
            font-weight: 700;
          }
          .line {
            flex: 1;
            margin-top: 6px;
            height: 1px;
            background-color: rgba(255, 255, 255, .2);
          }
        }
        &-content {
          padding: 24px 12px 0 12px;
        }
      }
      .bulletin {
        line-height: 24px;
        text-align: justify;
      }
    }
    .fade {
      &-enter-active {
        transition: all .3s ease;
      }
      &-leave-active {
        transition: all .5s cubic-bezier(1.0, 0.5, 0.8, 1.0);
      }
      &-enter,
      &-leave-active {
        opacity: 0;
      }
    }
  }
</style>
