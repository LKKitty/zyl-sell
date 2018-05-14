<template>
  <div class="header">
      <div class="content-wrapper">
        <div class="avatar">
          <img width="64" height="64" :src="seller.avatar" alt="">
        </div>
        <div class="content">
          <div class="title">
            <span class="brand">
            </span>
            <span class="name">
              {{seller.name}}
            </span>
          </div>
          <div class="description">
            {{seller.description}}/{{seller.deliveryTime}}分钟送达
          </div>
          <div v-if="seller.supports" class="supports">
            <span class="icon" :class="classMap[seller.supports[1].type]"></span>
            <span class="text">{{seller.supports[0].description}}</span>
          </div>
        </div>
        <div class="support-count" v-if="seller.supports">
          <span class="count">{{seller.supports.length}}个</span>
          <i class="icon-keyboard_arrow_right"></i>
        </div>
      </div>
      <div class="bulletin-warpper">
      </div>
  </div>
</template>

<script type="text/ecmascript-6">
export default {
  props: {
    seller: {
      type: Object
    }
  },
  created() {
    this.classMap = ['decrease', 'discount', 'guarantee', 'invoice', 'special'];
  },
  data() {
    return {
    };
  },
  components: {

  }
};
</script>

<style lang="stylus" >
@import "../../common/stylus/mixin";
  .header
    color: #fff;
    background : #9a9999;
    .content-wrapper
      position :relative;
      padding: 48px 24px 36px 48px;
      font-size : 0;
      .avatar
         display: inline-block;
         vertical-align : top ;
         img
          border-radius:4px;
      .content
        display: inline-block;
        font-size 28px;
        margin-left 32px;
        .title
          margin: 4px 0 8px 0;
          .brand
            display: inline-block;
            vertical-align : top ;
            width: 60px;
            height: 36px;
            bg-image('brand');
            background-size : 60px 36px;
            background-repeat : no-repeat;
          .name
            margin-left : 12px;
            font-size : 32px;
            font-weight :bold;
            line-height :32px;
        .description
          vertical-align :middle;
          margin-bottom :20px;
          line-height : 24px;
          font-size : 24px;
        .supports
          .icon
            display:inline-block;
            width: 24px;
            height: 24px;
            margin-left: 8px;
            background-size: 24px 24px;
            background-repeat: no-repeat;
            &.decrease
              bg-image('decrease_1');
            &.discount
              bg-image('discount_1');
            &.guarantee
              bg-image('guarantee_1');
            &.invoice
              bg-image('invoice_1');
            &.special
              bg-image('special_1');
          .text
            line-height :24px;
            font-size : 24px;
      .support-count
        position:absolute;
        right :24px;
        bottom :36px;
        padding:24px 36px;
        height :24px;
        line-height :24px;
        border-radius :24px;
        background :rgba(0,0,0,0.2);
        text-align :center;
        .count
          vertical-align :top;
          font-size :20px;
        .icon-keyboard_arrow_right
          margin-left:4px;
          line-height 24px;
          font-size : 20px;
  </style>
