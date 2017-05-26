<template>
  <div class="shopcart">
    <div class="shopcart-content">
      <div class="shopcart-content-left">
        <div class="logo">
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
    <div class="shopcart-list"></div>
  </div>
</template>

<script type="text/ecmascript-6">
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
        foodList: []
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
      }
    }
  }
</script>

<style lang="scss" rel="stylesheet/scss">
  .shopcart {
    position: absolute;
    bottom: -48px;
    left: 0;
    right: 0;
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
          &.check {
            color: #fff;
            background-color: #00b43c;
          }
        }
      }
    }
  }
</style>
