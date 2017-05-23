<template>
  <div class="star">
    <span v-for="" class="star-item" :class="itemSize"></span>
  </div>
</template>

<script type="text/ecmascript-6">
  const LENGTH = 5;

  export default {
    props: {
      size: {
        type: Number
      },
      score: {
        type: Number
      }
    },
    data () {
    },
    computed: {
      itemSize () {
          return 'star' + this.size
      }
    }
  }
</script>

<style lang="scss" rel="stylesheet/scss">
  .star {
    font-size: 0;
    &-item {
      display: inline-block;
    }
  }
</style>
