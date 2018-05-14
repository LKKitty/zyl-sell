<template>
  <div class="goods">
      <div class="menu-wrapper">
        <ul>
          <li v-for="item in goods" :key="item.id" class="menu-item">
              <span class="text border-1px">
                <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
              </span>
          </li>
        </ul>
      </div>
      <div class="foods-wrapper">
        <ul>
          <li v-for="item in goods" :key="item.id" class="food-list">
            <h1 class="titile">{{item.name}}</h1>
            <ul>
              <li v-for="food in item.foods" :key="food.id" class="food-item">
                <div class="icon">
                  <img src="food.icon"/>
                </div>
                <div class="content">
                  <h2 class="name">{{food.name}}</h2>
                  <p class="desc">food.description</p>
                  <div class="extra">
                    <span>月售{{food.sellCount}}份</span>
                     <span>好评率{{food.sellCount}}%</span>
                  </div>
                  <div class="price">
                    <span>￥{{food.price}}</span>
                     <span v-show="food.oldPrice">￥</span>
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
      goods: []
    };
  },
  components: {},
  created() {
    this.classMap = ["decrease", "discount", "guarantee", "invoice", "special"];
    this.$_http
      .get("/api/goods")
      .then(response => {
        response = response.data;
        console.log(response);
        if (response.errno === ERR_OK) {
          this.goods = response.data;
          console.log(this.goods);
        }
      })
      .catch(error => {
        console.log(error);
      });
  }
};
</script>

<style lang="stylus" scoped>
@import '../../common/stylus/mixin';

.goods {
  display: flex;
  position: absolute;
  width: 100%;
  top: 348px;
  bottom: 92px;
  overflow: hidden;

  .menu-wrapper {
    flex: 0 0 160px;
    width: 240px;
    background: #f3f5f7;

    .menu-item {
      display: table;
      height: 108px;
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
  }
}
</style>
