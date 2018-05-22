<template>
  <div class="goods">
      <div class="menu-wrapper"  ref="menuWrapper">
        <ul>
          <li v-for="item in goods" :key="item.id" class="menu-item" >
              <span class="text border-1px">
                <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
              </span>
          </li>
        </ul>
      </div>
      <div class="foods-wrapper"  ref="foodsWrapper">
        <ul>
          <li v-for="item in goods" :key="item.id" class="food-list food-list-hook" :class="{'current':currentIndex===$index}">
            <h1 class="title">{{item.name}}</h1>
            <ul>
              <li v-for="food in item.foods" :key="food.id" class="food-item border-1px">
                <div class="icon">
                  <img width="57" height="57" :src="food.icon"/>
                </div>
                <div class="content">
                  <h2 class="name">{{food.name}}</h2>
                  <p class="desc">{{food.description}}</p>
                  <div class="extra">
                    <span class="count">月售{{food.sellCount}}份</span>
                    <span>好评率{{food.sellCount}}%</span>
                  </div>
                  <div class="price">
                    <span class="now">￥{{food.price}}</span>
                     <span v-show="food.oldPrice" class="old">￥{{food.oldPrice}}</span>
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
import BScroll from 'better-scroll';
const ERR_OK = 0;
export default {
  name: "goods",
  props: {
    seller: {
      type: Object
    }
  },
  data() {
    return {
      goods: [],
      listHeight: [],
      scrollY: 0
    };
  },
  computed: {
    currentIndex() {
      for(let i=0;i<this.listHeight.length;i++){
        let height1 = this.listHeight[i];
        let height2 = this.listHeight[i+1];
        if (!height2 || (this.scrollY > height && this.scrollY <height2)) {
          return i;
        }
      }
    }
  },
  created() {
    this.classMap = ["decrease", "discount", "guarantee", "invoice", "special"];
    this.$_http
      .get("/api/goods")
      .then(response => {
        response = response.data;
        console.log(response);
        if (response.errno === ERR_OK) {
          this.goods = response.data;
          // console.log(this.goods);
         this.$nextTick(() => {
          this._initScroll();
          this._calculateHeight();
        });
        }
      })
      .catch(error => {
        console.log(error);
      });
  },
  methods: {
    _initScroll () {
      this.meunScroll = new BScroll(this.$refs.menuWrapper, {
        click: true
      });

      this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {
        click: true,
        probeType:3
      });
      this.foodsScroll.on('scroll',(pos) => {
        this.scrollY = Math.abs(Math.round(pos.y));
      })
    },
    _calculateHeight () {
      let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
      let height = 0;
      this.listHeight.push(height);
      for (let i = 0;i<foodList.length;i++){
        let item = foodList[i];
        height += item.clientHeight;
        this.listHeight.push(height);
      }
    }
  }
};
</script>

<style lang="stylus" scoped>
@import '../../common/stylus/mixin';

.goods {
  display: flex;
  position: absolute;
  width: 100%;
  top: 326px;
  bottom: 92px;
  overflow: hidden;

  .menu-wrapper {
    flex: 0 0 160px;
    width: 240px;
    background: #f3f5f7;

    .menu-item {
      display: table;
      height: 158px;
      width: 112px;
      line-height: 36px;
      padding: 0 24px;

      .icon {
        display: inline-block;
        width: 24px;
        height: 24px;
        margin-right: 8px;
        background-size: 24px 24px;
        background-repeat: no-repeat;

        &.decrease {
          bg-image('decrease_3');
        }

        &.discount {
          bg-image('discount_3');
        }

        &.guarantee {
          bg-image('guarantee_3');
        }

        &.invoice {
          bg-image('invoice_3');
        }

        &.special {
          bg-image('special_3');
        }
      }

      .text {
        display: table-cell;
        width: 112px;
        vertical-align: middle;
        border-1px(rgba(7, 17, 27, 0.1));
        font-size: 24px;
      }
    }
  }

  .foods-wrapper {
    background: #ffffff;
    flex: 1;
    .title{
      padding-left 24px
      height 82px
      line-height 82px
      border-left 4px soild #d9dde1
      font-size 36px
      color rgb(147,153,159)
      background-color #f3f5f7
      }
    .food-item{
      display flex
      margin 36px
      padding-bottom 36px
      border-1px(rgba(7, 17, 27, 0.1))
      &:last-child{
        border-none()
        margin-bottom 0
        }
      }
      .icon{
        flex 0 0 114px
        margin-right 20px
        }
      .content{
        flex 1
        .name{
          margin 4px 0 16px 0
          height 28px;
          line-height 28px;
          font-size 28px
          color rgb(7,17,27);
          }
        .desc,.extra{
          line-height 20px
          font-size 20px;
          color rgb(147,153,159)
          }
        .desc{
          margin-bottom 16px
          line-height 34px
          }
        .count{
          margin-right 24px
          }
        .price{
          font-weight 700
          line-height 48px;
          .now{
            margin-right 16px
            font-size 28px
            color rgb(240,20,20)
            }
          .old{
            text-decoration line-through;
            font-size  20px;
            color rgb(147,153,159)
            }
          }
    }
 }
}
</style>
