<template>
  <div class="goods">
    <div class="menu-wrap" ref="menuWrap">
      <ul>
        <li class="menu-item menu-item-hook" @click="selectMenu(index, $event)" v-for="(item, index) in goods"
            :class="{'current': currentIndex === index}">
          <span class="text">
            <i v-show="item.type > 0" class="icon" :class="classMap[item.type]"></i>
            {{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrap" ref="foodsWrap">
      <ul>
        <li v-for="item in goods" class="food-list food-list-hook">
          <h2 class="title">{{item.name}}</h2>
          <ul>
            <li v-for="food in item.foods" class="food-item" @click="selectFood(food,$event)">
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
                    <span class="cur-price"><small>&yen;</small>{{food.price}}</span>
                    <span v-show="food.oldPrice" class="old-price">&yen;{{food.oldPrice}}</span>
                  </div>
                  <div class="cartcontrol-wrap">
                    <cartcontrol :food="food"></cartcontrol>
                  </div>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <shopcart :select-foods="selectFoods"
              :delivery-price="seller.deliveryPrice"
              :min-price="seller.minPrice"></shopcart>
    <food :food="selectedFood" ref="food"></food>
  </div>
</template>

<script type="text/ecmascript-6">
  import BScroll from 'better-scroll'
  import shopcart from '@/components/shopcart/shopcart'
  import cartcontrol from '@/components/cartcontrol/cartcontrol'
  import food from '@/components/food/food'

  const ERR_OK = 0
  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data () {
      return {
        goods: {},
        classMap: ['decrease', 'discount', 'special', 'invoice', 'guarantee'],
        listHeight: [],
        scrollY: 0,
        selectedFood: {}
      }
    },
    created () {
      this.$http.get('api/goods').then(response => {
        response = response.body
        if (response.errno === ERR_OK) {
          this.goods = response.data
          this.$nextTick(() => {
            this._initScroll()
            this._calculateHeight()
          })
        }
      })
    },
    computed: {
      currentIndex () {
        for (let i = 0; i < this.listHeight.length; i++) {
          let height1 = this.listHeight[i]
          let height2 = this.listHeight[i + 1]
          if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
            return i
          }
        }
        return 0
      },
      selectFoods () {
        let foods = []
        this.goods.forEach(good => {
          good.foods.forEach(food => {
            if (food.count) {
              foods.push(food)
            }
          })
        })
        return foods
      }
    },
    methods: {
      _initScroll () {
        this.menuScroll = new BScroll(this.$refs.menuWrap, {
          click: true
        })
        this.foodsScroll = new BScroll(this.$refs.foodsWrap, {
          click: true,
          probeType: 3
        })
        this.foodsScroll.on('scroll', (pos) => {
          this.scrollY = Math.abs(Math.round(pos.y))
        })
      },
      _calculateHeight () {
        let foodList = this.$refs.foodsWrap.getElementsByClassName('food-list-hook')
        let height = 0
        this.listHeight.push(height)
        for (let i = 0; i < foodList.length; i++) {
          height += foodList[i].clientHeight
          this.listHeight.push(height)
        }
      },
      selectMenu (index, event) {
        if (!event._constructed) {
          return
        }
        let foodList = this.$refs.foodsWrap.getElementsByClassName('food-list-hook')
        let el = foodList[index]
        this.foodsScroll.scrollToElement(el, 300)
      },
      selectFood (food, event) {
        if (!event._constructed) {
          return
        }
        this.selectedFood = food
        this.$refs.food.show()
      }
    },
    components: {
      shopcart,
      cartcontrol,
      food
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
    .menu {
      &-wrap {
        flex: 0 0 80px;
        width: 80px;
        background-color: #f3f5f7;
        overflow: hidden;
      }
      &-item {
        position: relative;
        box-sizing: border-box;
        width: 100%;
        height: 54px;
        padding: 0 12px;
        display: table;
        line-height: 14px;
        & + .menu-item {
          &:after {
            content: '';
            position: absolute;
            top: 0;
            left: 12px;
            right: 12px;
            height: 1px;
            background-color: rgba(7, 17, 27, .1);
          }
        }
        &.current {
          background-color: #fff;
        }
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
        overflow: hidden;
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
          padding-bottom: 10px;
          &-left {
            flex: 0 0 57px;
            width: 57px;
            height: 57px;
            margin-right: 10px;
          }
          &-body {
            position: relative;
            flex: 1;
          }
          .name {
            margin-top: 2px;
            font-size: 14px;
            color: rgb(7, 17, 27);
            font-weight: 200;
          }
          .description,
          .extra {
            margin-top: 8px;
            font-size: 10px;
            color: rgb(147, 153, 159);
          }
          .price {
            margin-top: 8px;
          }
          .cur-price {
            margin-right: 6px;
            font-size: 14px;
            line-height: 24px;
            font-weight: 700;
            color: rgb(240, 20, 20);
            small {
              font-size: 10px;
              font-weight: 200;
            }
          }
          .old-price {
            font-size: 10px;
            color: rgb(147, 153, 159);
          }
          .cartcontrol-wrap {
            position: absolute;
            right: 0;
            bottom: -6px;
          }
        }
      }
    }
  }
</style>
