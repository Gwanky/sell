<template>
  <div class="goods">
    <div class="menu-wrap" ref="menuWrap">
      <ul>
        <li class="menu-item" v-for="item in goods">
          <span class="text">
            <i v-show="item.type > 0" class="icon" :class="classMap[item.type]"></i>
            {{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrap" ref="foodsWrap">
      <ul>
        <li v-for="item in goods" class="food-list">
          <h2 class="title">{{item.name}}</h2>
          <ul>
            <li v-for="food in item.foods" class="food-item">
              <div class="media">
                <div class="media-left">
                  <img :src="food.icon" width="57" height="57" alt="">
                </div>
                <div class="media-body">
                  <h3 class="name">{{food.name}}</h3>
                  <p class="description">{{food.description}}</p>
                  <p class="extra">
                    <span class="count">月售{{food.sellCount}}份</span>
                    <span>好评率{{food.rating}}</span>
                  </p>
                  <div class="price">
                    <span class="cur-price">&yen;{{food.price}}</span>
                    <span v-show="food.oldPrice" class="old-price">&yen;{{food.oldPrice}}</span>
                  </div>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import BScroll from 'better-scroll'

  const ERR_OK = 0
  export default {
    data () {
      return {
        goods: {},
        classMap: []
      }
    },
    created () {
      this.$http.get('api/goods').then(response => {
        response = response.body
        if (response.errno === ERR_OK) {
          this.goods = response.data
          this.$nextTick(() => {
            this._initScroll()
          })
        }
      })
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
    },
    methods: {
      _initScroll () {
        this.menuScroll = new BScroll(this.$refs.menuWrap, {})
        this.foodsScroll = new BScroll(this.$refs.foodsWrap, {})
      }
    }
  }
</script>

<style lang="scss" rel="stylesheet/scss">
  @import "../../common/sass/mixin";

  .goods {
    position: absolute;
    top: 175px;
    left: 0;
    right: 0;
    bottom: 48px;
    display: flex;
    overflow: hidden;
    .menu {
      &-wrap {
        flex: 0 0 80px;
        width: 80px;
        background-color: #f3f5f7;
      }
      &-item {
        box-sizing: border-box;
        width: 100%;
        height: 54px;
        padding: 0 12px;
        display: table;
        line-height: 14px;
        .text {
          display: table-cell;
          vertical-align: middle;
        }
        .icon {
          display: inline-block;
          width: 12px;
          height: 12px;
          background-size: 12px 12px;
          vertical-align: middle;
          &.decrease {
            @include bg-img('decrease_3');
          }
          &.discount {
            @include bg-img('discount_3');
          }
          &.special {
            @include bg-img('special_3');
          }
          &.invoice {
            @include bg-img('invoice_3');
          }
          &.guarantee {
            @include bg-img('guarantee_3');
          }
        }
      }
    }
    .foods {
      &-wrap {
        flex: 1;
      }
    }
    .food {
      &-list {
        .title {
          padding-left: 14px;
          font-size: 12px;
          line-height: 26px;
          color: rgb(147, 153, 159);
          border-left: 2px solid #d9dde1;
          background-color: #f3f5f7;
        }
      }
      &-item {
        position: relative;
        + .food-item:before {
          content: '';
          position: absolute;
          top: 0;
          left: 18px;
          right: 18px;
          height: 1px;
          background-color: rgba(7, 17, 27, .1);
        }
        .media {
          display: flex;
          padding: 18px;
          &-left {
            flex: 0 0 57px;
            width: 57px;
            height: 57px;
            margin-right: 10px;
          }
          &-body {
            flex: 1;
          }
        }
      }
    }
  }
</style>
