<template>
  <div id="app">
   <v-header :seller = "seller"></v-header>
   <div class="tab" border-1px>
     <router-link to="/goods"  class="tab-item">商品</router-link>
     <router-link to="/ratings" class="tab-item">评论</router-link>
     <router-link to="/seller" class="tab-item">商家</router-link>
   </div>
    <router-view></router-view>
  </div>
</template>

<script type="text/ecmascript-6">
// import { urlParse } from 'common/js/util';
import header from 'components/header/header';
const ERR_OK = 0;
// const debug = process.env.NODE_ENV !== 'production';
export default {
  name: 'App',
  data() {
    return {
      seller: {}
    };
  },
  created() {
    const url = '/api/seller';
    // let that = this;
    this.$_http
      .get(url + '?id=' + this.seller.id)
      .then((response) => {
        response = response.data;
        // console.log(response.errno);
      if (response.errno === ERR_OK) {
        this.seller = Object.assign({}, this.seller, response.data);
      }
      })
      .catch((error) => {
        console.log(error);
      });
  },
  components: {
    'v-header': header
  }
};
</script>

<style lang="stylus" scoped>
@import './common/stylus/mixin.styl';

#app {
  .tab {
    display: flex;
    width: 100%;
    height: 80px;
    line-height: 80px;
    border-1px: rgba(7, 17, 27, 0.1);

    .tab-item {
      flex: 1;
      text-align: center;
      display: block;
      font-size: 14px;
      color: rgb(77, 85, 93);

      &.active {
        color: rgb(240, 20, 0);
      }
    }
  }
}
</style>
