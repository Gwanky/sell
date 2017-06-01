<template>
  <div class="cartcontrol">
    <transition name="move">
      <span v-show="food.count > 0" class="cart-decrease" @click.stop.prevent="decreaseCart">
        <i class="inner fa fa-minus-circle"></i>
      </span>
    </transition>
    <span v-show="food.count > 0" class="text">{{food.count}}</span>
    <span class="cart-add" @click.stop.prevent="addCart">
      <i class="fa fa-plus-circle"></i>
    </span>
  </div>
</template>

<script type="text/ecmascript-6">
  import Vue from 'vue'
  export default {
    props: {
      food: {
        type: Object
      }
    },
    methods: {
      addCart (event) {
        if (!event._constructed) {
          return
        }
        if (!this.food.count) {
          Vue.set(this.food, 'count', 1)
        } else {
          this.food.count++
        }
      },
      decreaseCart (event) {
        if (!event._constructed) {
          return
        }
        this.food.count--
      }
    }
  }
</script>

<style lang="scss" rel="stylesheet/scss">
  .cartcontrol {
    font-size: 0;
    .cart-decrease,
    .cart-add,
    .text {
      display: inline-block;
    }
    .cart-decrease,
    .cart-add {
      padding: 6px;
      font-size: 24px;
      line-height: 1;
      font-weight: 200;
      color: rgb(0, 160, 240);
    }
    .text {
      width: 20px;
      text-align: center;
      padding-top: 6px;
      vertical-align: top;
      font-size: 10px;
      color: rgb(147, 153, 159);
      line-height: 24px;
    }
    .move-enter-active,
    .move-leave-active {
      transform: translate3D(0,0,0);
      transition: all .8s linear;
      .inner {
        transition: all 1s ease;
      }
    }
    .move-enter,
    .move-leave-active{
      opacity: 0;
      transform: translateX(12px);
      .inner {
        transform: rotate(180deg);
      }
    }
  }
</style>
