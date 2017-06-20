<template>
  <div class="ratings" ref="ratings">
    <div class="ratings-content">
      <div class="overview">
        <div class="overview-left">
          <div class="score">{{seller.score}}</div>
          <div class="lab">综合评分</div>
          <div class="desc">高于周边商家{{seller.rankRate}}%</div>
        </div>
        <div class="overview-right">
          <ul class="list">
            <li class="item">
              <div class="item-lab">服务态度</div>
              <div class="item-val">
                <div class="star-wrap">
                  <star :size="36" :score="seller.serviceScore"></star>
                </div>
              </div>
              <div class="item-text">{{seller.serviceScore}}</div>
            </li>
            <li class="item">
              <div class="item-lab">食物得分</div>
              <div class="item-val">
                <div class="star-wrap">
                  <star :size="36" :score="seller.foodScore"></star>
                </div>
              </div>
              <div class="item-text">{{seller.foodScore}}</div>
            </li>
            <li class="item">
              <div class="item-lab">送达时间</div>
              <div class="item-val">{{seller.deliveryTime}}分钟</div>
            </li>
          </ul>
        </div>
      </div>

      <split></split>

      <ratingselect
        @select="selectRating"
        @toggle="toggleContent"
        :ratings="ratings"
        :only-content="onlyContent"
        :select-type="selectType"
        :desc="desc"></ratingselect>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import star from '@/components/star/star'
  import split from '@/components/split/split'
  import ratingselect from '@/components/ratingselect/ratingselect'
  import BScroll from 'better-scroll'

  const ALL = 2
  const ERR_OK = 0

  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data () {
      return {
        ratings: [],
        selectType: ALL,
        onlyContent: true,
        desc: {
          positive: '满意',
          negative: '不满意',
          all: '全部'
        }
      }
    },
    created () {
      this.$http.get('api/ratings').then(response => {
        response = response.body
        if (response.errno === ERR_OK) {
          this.ratings = response.data
        }
      })
    },
    methods: {
      _initScroll () {
        this.scroll = new BScroll(this.els.ratings, {
          click: true
        })
      },
      toggleContent () {
        this.onlyContent = !this.onlyContent
        this.$nextTick(() => {
          this.scroll.refresh()
        })
      },
      selectRating (type) {
        this.selectType = type
        this.$nextTick(() => {
          this.scroll.refresh()
        })
      }
    },
    components: {
      star,
      split,
      ratingselect
    }
  }
</script>

<style lang="scss" rel="stylesheet/scss">
  .ratings {
    .overview {
      display: flex;
      padding-top: 18px;
      padding-bottom: 18px;
      &-left {
        width: 35%;
        text-align: center;
        border-right: 1px solid rgba(7, 17, 27, .1);
        .score {
          margin-bottom: 6px;
          font-size: 24px;
          color: rgb(255, 153, 0);
          line-height: 28px;
        }
        .lab {
          margin-bottom: 8px;
          color: rgb(7, 17, 27);
        }
        .desc {
          font-size: 10px;
          color: rgb(147, 153, 157);
        }
      }
      &-right {
        flex: 1;
        padding-left: 24px;
        .item {
          display: flex;
          + {
            .item {
              margin-top: 8px;
            }
          }
          &-lab {
            width: 4em;
            margin-right: 12px;
            line-height: 18px;
            color: rgb(7, 17, 27);
          }
          &-val {
            color: rgb(147, 153, 159);
            line-height: 18px;
          }
          &-text {
            margin-left: 12px;
            line-height: 18px;
            color: rgb(255, 153, 0);
          }
        }
      }
    }
  }
</style>
