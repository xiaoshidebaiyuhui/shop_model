<style lang="scss" scoped>
@import "~@/css/mixin";
@import "~@/css/var";
.c-shopcart {
  &.in-tab {
    .padding-wrap {
      padding-bottom: 1.2rem;
    }

    .item_page_footer {
      bottom: 0.55rem;
    }
  }

  .padding-wrap {
    padding-bottom: 0.75rem;
  }

  .list {
    background-color: #fff;
    // margin-top: -1px;
    margin-bottom: 0.4rem;
    @include border-top();
    padding: 0 0.15rem;
  }

  .item {
    @include flexbox;
    padding: 0.15rem 0;
    @include border-bottom();

    .remove-btn {
      float: right;
      font-size: 0.2rem;
    }

    .item-checkbox-wrap {
      display: inline-block;
      align-self: stretch;
      @include flexbox;
    }

    .item-img {
      align-self: flex-start;
      width: 0.9rem;
      height: 0.9rem;
      margin-left: 0.1rem;
      margin-right: 0.1rem;
      border: 1px solid #eee;
    }

    .item-content {
      margin-top: -0.02rem;
      @include flex;
    }

    .item-name {
      font-size: 0.13rem;
      font-weight: 500;
      // color: #444;
      max-height: 0.34rem;
      line-height: 0.17rem;
      overflow: hidden;
    }

    .item-price {
      color: $color-primary;
      font-weight: 600;
    }

    .item-prop-wrap {
      margin-top: 0.02rem;
      color: #777;
    }

    .tag-wrap{
      .c-tag{
        margin-top: 0.05rem;
      }
      
    }

    .item-prop {
      position: relative;
      background: #f5f5f5;
      color: #999;
      border-radius: 0.03rem;
      padding: 0.04rem 0.05rem;
      font-size: 0.12rem;
      display: inline-block;
      // padding-right: 0.18rem;

      // &::after {
      //   $size: 0.07rem;
      //   content: "";
      //   position: absolute;
      //   top: 50%;
      //   right: 0.08rem;
      //   transform: translateY(-50%) rotate(45deg);
      //   width: $size;
      //   height: $size;
      //   border: 1px solid #bbb;
      //   border-left: 0;
      //   border-top: 0;
      // }
    }

    .item-bottom {
      padding-top: 0.05rem;
      @include flexbox(space-between);
    }
  }

  .item_page_footer {
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    height: pxTorem(100);
    // border-top: 1px solid #eaeaea;
    @include border-top();
    background: rgba(255, 255, 255, 1);
    z-index: 3;
    .item_page_footer_content {
      width: 100%;
      box-sizing: border-box;
      height: 100%;

      .item_page_footer_follow_wrap {
        @include flexbox(space-between);
        @include flex;
      }
      .amount-wrap {
        padding-right: 0.12rem;
        text-align: right;
      }

      .amount {
        padding-top: 0.02rem;
      }

      .amount-strong {
        font-size: 0.15rem;
        font-weight: 600;
        color: $color-primary;
      }

      .hint {
        font-size: 0.1rem;
        color: #999;
      }

      .footer-btn {
        border: none;
        color: #fff;
        height: 0.5rem;
        min-width: 0.9rem;
        background: $color-primary-gradient;
        // border: 1px solid $color-primary;

        &:disabled {
          background: $color-primary-gradient-disabled;
          // border: 1px solid $color-primary-disabled;
        }

        &:not(:disabled):active {
          background: $color-primary-gradient-active;
          // border: 1px solid $color-primary-active;
        }
      }
    }
  }
}
</style>

<template>
  <div class="c-shopcart" :class="{'in-tab':inTab}">
    <c-header :title="'购物车'" :backType="inTab? 0 : 1"></c-header>
    <div class="c-page-body">
      <div class="padding-wrap c-header-pd">
        <div v-if="!isLogin" style="padding-top:0.4rem;text-align:center;">亲~ 登录后才可以查看购物车
          <div style="padding-top:0.2rem;">
            <router-link tag="button" to="/login" class="c-btn btn-primary">去登录</router-link>
          </div>
        </div>
        <div v-else-if="list && list.length > 0">
          <div class="list" style="margin-top:-1px;">
            <div class="item" v-for="(shopcart,index) in list" :key="index">
              <label class="item-checkbox-wrap">
                <c-checkbox v-model="checkedFlags[shopcart.id]"></c-checkbox>
              </label>
              <img class="item-img" v-lazy="shopcart.item.imgList[0]" alt @click="$router.push(`/items/${shopcart.item.id}`)">
              <div class="item-content">
                <router-link
                  tag="div"
                  :to="`/items/${shopcart.item.id}`"
                  class="item-name"
                >{{shopcart.item.name}}</router-link>
                <div class="item-prop-wrap">
                  <div class="item-prop">{{shopcart.sku.propvalueTextList}}</div>
                </div>
                <div class="tag-wrap">
                  <span class="c-tag" v-if="shopcart.flash && shopcart.flash.status == 1">限时特价</span>
                </div>
                <div class="item-bottom">
                  <div class="item-price" v-if="shopcart.flash && shopcart.flash.status == 1">￥{{shopcart.flash.sku.flashPrice}}</div>
                  <div class="item-price" v-else>￥{{shopcart.sku.price}}</div>
                  <c-number-input
                    v-model="shopcart.quantity"
                    :min="1"
                    @input="updateShopcart(shopcart.id,$event)"
                  ></c-number-input>

                  <!-- <i class="iconfont icon-add1"></i> -->
                </div>
                <div
                  style="padding-top: 15px;
                  margin-bottom: -15px;
                  overflow: hidden;
                  color: #ccc;"
                >
                  <i
                    class="iconfont icon-close_light remove-btn"
                    @click="removeShopcart(shopcart.id)"
                  ></i>
                </div>
              </div>
            </div>
          </div>

        </div>

        <c-empty-hint v-else icon="icon-cart_light" hint="您的购物车是空的哦！">
          <button v-if="inTab" class="c-btn btn-primary" @click="$emit('gotoHome')">随便逛逛</button>
        </c-empty-hint>
        <c-recommend-list ref="recommend" cacheId="recommend" style="margin-top:0.5rem;"></c-recommend-list>
      </div>
    </div>
    <div class="item_page_footer">
      <div class="item_page_footer_content chen_center_absolute">
        <div class="item_page_footer_follow_wrap">
          <label>
            <c-checkbox v-model="allChecked" style="margin-left:0.1rem;margin-right:0.08rem;"></c-checkbox>全选
          </label>
          <div class="amount-wrap">
            <div class="amount">
              合计:
              <span class="amount-strong">￥{{checkedAmount}}</span>
            </div>
            <div class="hint">不含运费</div>
          </div>
        </div>
        <button
          :disabled="checkedCount === 0"
          class="footer-btn"
          @click="submit"
        >结算({{checkedCount}})</button>
        <!-- <div class="chen_center_absolute_center item_page_footer_buys_wrap"> -->
        <!-- </div> -->
      </div>
    </div>
  </div>
</template>


<script>
import Vue from "vue";
import services from "@/services";
import routerCacheComponent from "@/routerCache/component";

export default {
  mixins: [
    routerCacheComponent({
      scrollWrapSelector: ".c-page-body"
    })
  ],
  props: {
    inTab: {
      type: Boolean,
      default: true
    }
  },
  data() {
    return {
      isLogin: false,
      checkedFlags: {},
      list: []
    };
  },
  computed: {
    allChecked: {
      get() {
        let flags = Object.values(this.checkedFlags);
        return flags.length > 0 && flags.every(item => item);
      },
      set(val) {
        for (let key in this.checkedFlags) {
          this.checkedFlags[key] = val;
        }
      }
    },
    checkedItems() {
      return this.list.filter(item => this.checkedFlags[item.id]);
    },
    checkedCount() {
      return Object.values(this.checkedFlags).filter(item => item).length;
    },
    checkedAmount() {
      return this.checkedItems.reduce((prev, current) => {
        let price = current.flash &&  current.flash.status == 1? current.flash.sku.flashPrice : current.sku.price;
        return prev + price * current.quantity;
      }, 0);
    }
  },
  methods: {
    async fetchShopcartList() {
      try {
        let res = await services.fetchShopcartList();

        if (services.$isError(res)) throw new Error(res.message);

        this.list = res.data;

        this.checkedFlags = {};
        this.list.forEach(item => {
          Vue.set(this.checkedFlags, item.id, false);
        });
      } catch (err) {
        return this.$toast(err.message);
      }
    },

    async updateShopcart(id, quantity) {
      try {
        let res = await services.updateShopcart({
          shopcartId: id,
          quantity
        });

        if (services.$isError(res)) throw new Error(res.message);
      } catch (err) {
        return this.$toast(err.message);
      }
    },

    //删除购物车
    async removeShopcart(id) {
      try {
        let res = await services.removeShopcart({
          shopcartId: id
        });

        if (services.$isError(res)) throw new Error(res.message);

        let index = this.list.findIndex(item => item.id == id);
        this.list.splice(index, 1);
        Vue.delete(this.checkedFlags,id);
        this.$nextTick();
        this.$toast(res.message);
      } catch (err) {
        return this.$toast(err.message);
      }
    },

    submit() {
      console.log(this.checkedItems);

      let queryData = [];

      this.checkedItems.forEach(shopcart => {
        let { id, itemId, skuId, quantity } = shopcart;
        queryData.push({
          shopcartId: id,
          itemId,
          skuId,
          quantity
        });
      });

      queryData = JSON.stringify(queryData);

      this.$router.push({ path: "/confirmorder", query: { p: queryData } });
    }
  },
  created() {
    this.isLogin = services.$isLogin();

    if (this.isLogin) {
      this.fetchShopcartList();
    }
  }
};
</script>
