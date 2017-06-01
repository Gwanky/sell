<template>
  <div class="ratingselect">
    <div class="rating-type">
      <span @click="select(2,$event)" class="block positive" :class="{active: selectType === 2}">{{desc.all}}<small>{{ratings.length}}</small></span>
      <span @click="select(0,$event)" class="block positive" :class="{active: selectType === 0}">{{desc.positive}}<small>{{positive.length}}</small></span>
      <span @click="select(1,$event)" class="block negative" :class="{active: selectType === 1}">{{desc.negative}}<small>{{negative.length}}</small></span>
    </div>
    <div class="switch" @click="toggleContent">
      <i class="fa fa-check-circle" :class="{on: onlyContent}"></i>
      <span class="text">只看有内容的评价</span>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  const POSITIVE = 0
  const NEGATIVE = 1
  const ALL = 2
  export default {
    props: {
      ratings: {
        type: Array,
        default () {
          return []
        }
      },
      selectType: {
        type: Number,
        default: ALL
      },
      onlyContent: {
        type: Boolean,
        default: false
      },
      desc: {
        type: Object,
        default () {
          return {
            positive: '推荐',
            negative: '吐槽',
            all: '全部'
          }
        }
      }
    },
    computed: {
      positive () {
        return this.ratings.filter(rating => {
          return rating.rateType === POSITIVE
        })
      },
      negative () {
        return this.ratings.filter(rating => {
          return rating.rateType === NEGATIVE
        })
      }
    },
    methods: {
      select (type, event) {
        if (!event._constructed) {
          return
        }
        this.$emit('select', type)
      },
      toggleContent (event) {
        if (!event._constructed) {
          return
        }
        this.$emit('toggle')
      }
    }
  }
</script>

<style lang="scss" rel="stylesheet/scss">
  .ratingselect {
    .rating-type {
      position: relative;
      padding: 18px;
      font-size: 0;
      &:after {
        content: '';
        position: absolute;
        bottom: 0;
        left: 18px;
        right: 18px;
        height: 1px;
        background-color: rgba(7, 17, 27, .1);
      }
      .block {
        display: inline-block;
        margin-right: 8px;
        padding: 8px 12px;
        font-size: 12px;
        line-height: 16px;
        color: rgb(77, 85, 93);
        background-color: rgba(77, 85, 93, .2);
        &.positive {
          background-color: rgba(0, 160, 220, .2);
          &.active {
            color: #fff;
            background-color: rgba(0, 160, 220, 1);
          }
        }
        &.negative {
          background-color: rgba(77, 85, 93, .2);
          &.active {
            color: #fff;
            background-color: rgba(77, 85, 93, 1);
          }
        }
        small {
          font-size: 8px;
          margin-left: 4px;
        }
      }
    }
    .switch {
      padding: 12px 18px;
      font-size: 0;
      .fa,
      .text {
        display: inline-block;
        vertical-align: top;
        line-height: 24px;
        color: rgb(147, 153, 159);
      }
      .fa {
        margin-right: 4px;
        font-size: 20px;
        &.on {
          color: #00c850;
        }
      }
      .text {
        font-size: 12px;
      }
    }
  }
</style>
