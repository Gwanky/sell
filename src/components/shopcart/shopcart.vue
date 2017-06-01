<template>
  <div class="shopcart">
    <div class="shopcart-content">
      <div class="shopcart-content-left">
        <div class="logo" @click="toggleList">
          <div class="logo-wrap" :class="{'highline': totalCount > 0}"><i class="fa fa-shopping-cart"></i></div>
          <span v-show="totalCount > 0" class="count">{{totalCount}}</span>
        </div>
        <div class="total-price" :class="{'highline': totalCount > 0}">&yen;{{totalPrice}}</div>
        <div class="delivery-price">另需配送费&yen;{{deliveryPrice}}元</div>
      </div>
      <div class="shopcart-content-right">
        <div class="desc" :class="{'check': totalPrice >= minPrice}">{{payDesc}}</div>
      </div>
    </div>
    <transition name="fade">
      <div v-show="listShow" class="backdrop" @click="hideList"></div>
    </transition>
    <transition name="show">
      <div v-show="listShow" class="shopcart-list">
        <div class="head">
          <h2 class="title">购物车</h2>
          <span class="empty" @click="empty">清空</span>
        </div>
        <div class="content" ref="content">
          <ul>
            <li class="item" v-for="food in selectFoods">
              <div class="name">{{food.name}}</div>
              <div class="price">&yen;{{food.price*food.count}}</div>
              <div class="cartcontrol-wrap">
                <cartcontrol :food="food"></cartcontrol>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </transition>
  </div>
</template>

<script type="text/ecmascript-6">
  import BScroll from 'better-scroll'
  import cartcontrol from '@/components/cartcontrol/cartcontrol'

  export default {
    props: {
      minPrice: {
        type: Number,
        default: 0
      },
      deliveryPrice: {
        type: Number,
        default: 0
      },
      selectFoods: {
        type: Array
      }
    },
    data () {
      return {
        foodList: [],
        fold: true
      }
    },
    computed: {
      totalPrice () {
        let total = 0
        for (let i = 0; i < this.selectFoods.length; i++) {
          total += this.selectFoods[i].price * this.selectFoods[i].count
        }
        return total
      },
      totalCount () {
        let count = 0
        for (let i = 0; i < this.selectFoods.length; i++) {
          count += this.selectFoods[i].count
        }
        return count
      },
      payDesc () {
        let diff = this.minPrice - this.totalPrice
        if (diff > 0) {
          return `还差${diff}元起送`
        } else if (diff <= 0) {
          return '去结算'
        } else {
          return `&yen;${this.minPrice}元起送`
        }
      },
      listShow () {
        if (!this.totalCount) {
          this.fold = true
          return false
        }
        let show = !this.fold
        if (show) {
          this.$nextTick(() => {
            if (!this.scroll) {
              this.scroll = new BScroll(this.$refs.content, {
                click: true
              })
            } else {
              this.scroll.refresh()
            }
          })
        }
        return show
      }
    },
    methods: {
      toggleList () {
        if (!this.totalCount) {
          return
        }
        this.fold = !this.fold
      },
      hideList () {
        this.fold = true
      },
      empty () {
        this.selectFoods.forEach(food => {
          food.count = 0
        })
      }
    },
    components: {
      cartcontrol
    }
  }
</script>

<style lang="scss" rel="stylesheet/scss">
  .shopcart {
    position: absolute;
    bottom: -48px;
    left: 0;
    right: 0;
    z-index: 1;
    height: 48px;
    &-content {
      display: flex;
      height: 100%;
      color: rgba(255, 255, 255, .4);
      background-color: #141d27;
      &-left {
        flex: 1;
        font-size: 0;
      }
      .logo,
      .total-price,
      .delivery-price {
        display: inline-block;
        vertical-align: top;
      }
      .logo {
        position: relative;
        top: -10px;
        box-sizing: border-box;
        width: 56px;
        height: 56px;
        padding: 6px;
        margin-left: 12px;
        border-radius: 50%;
        background-color: #141d27;
        &-wrap {
          width: 100%;
          height: 100%;
          font-size: 24px;
          line-height: 44px;
          border-radius: 50%;
          text-align: center;
          background-color: #202932;
          transition: background-color .5s;
          &.highline {
            color: #fff;
            background-color: #00a0dc;
          }
        }
      }
      .count {
        position: absolute;
        right: 0;
        top: 0;
        box-sizing: border-box;
        min-width: 24px;
        text-align: center;
        padding-left: 6px;
        padding-right: 6px;
        font-size: 9px;
        line-height: 16px;
        color: #fff;
        font-weight: 700;
        border-radius: 8px;
        background-color: rgb(240, 20, 20);
        box-shadow: 0 4px 8px 0 rgba(0, 0, 0, .4);
      }
      .total-price {
        margin: 12px 10px 0;
        font-size: 16px;
        font-weight: 700;
        line-height: 24px;
        &.highline {
          color: #fff;
        }
      }
      .delivery-price {
        margin-top: 12px;
        font-size: 10px;
        line-height: 24px;
        padding-left: 10px;
        border-left: 1px solid rgba(255, 255, 255, .1);
      }
      &-right {
        flex: 0 0 95px;
        width: 95px;
        .desc {
          display: block;
          width: 100%;
          font-size: 12px;
          font-weight: 700;
          text-align: center;
          line-height: 48px;
          background-color: #2b333b;
          transition: background-color .5s;
          &.check {
            color: #fff;
            background-color: #00b43c;
          }
        }
      }
    }
    &-list {
      position: absolute;
      top: 0;
      left: 0;
      z-index: -1;
      width: 100%;
      background-color: #fff;
      transform: translate3D(0, -100%, 0);
      transition: all .5s;
      .head {
        display: flex;
        padding: 0 6px 0 18px;
        line-height: 40px;
        background-color: #f3f5f7;
        border-bottom: 1px solid rgba(7, 17, 27, .1);
        .title {
          flex: 1;
          font-size: 14px;
          color: rgb(7, 17, 27);
        }
        .empty {
          display: inline-block;
          padding: 0 12px;
          color: rgb(0, 160, 220);
        }
      }
      .content {
        max-height: 215px;
        overflow: hidden;
        .item {
          position: relative;
          display: flex;
          padding: 6px 18px;
          .name,
          .price {
            font-size: 14px;
            line-height: 37px;
          }
          .name {
            flex: 1;
            color: rgb(7, 17, 27);
          }
          .price {
            color: rgb(240, 20, 20);
          }
          .cartcontrol-wrap {
            margin-left: 12px;
          }
        }
      }
    }
    .show {
      &-enter-active,
      &-leave-active {
        transform: translate3D(0, -100%, 0);
      }
      &-enter,
      &-leave-active {
        transform: translate3D(0, 0, 0);
      }
    }
    .backdrop {
      position: fixed;
      top: 0;
      left: 0;
      bottom: 0;
      right: 0;
      z-index: -1;
      background-color: rgba(7, 17, 27, .6);
      -webkit-backdrop-filter: blur(10px);
      transition: opacity .5s;
    }
    .fade {
      &-enter-active,
      &-leave-active {
        opacity: 1;
      }
      &-enter,
      &-leave-active {
        opacity: 0;
      }
    }
  }
</style>
