<template>
  <div class="bottom-bar">
    <div class="bar-item bar-left">
      <div>
        <i class="icon service"></i>
        <span class="text">客服</span>
      </div>
      <div>
        <i class="icon shop"></i>
        <span class="text">店铺</span>
      </div>
      <div @click="isCollect = !isCollect">
        <i class="icon" :class="{collectActive: isCollect}"></i>
        <span class="text" :class="{textActive: isCollect}">{{collectClick()}}</span>
      </div>
      <div @click="cartClick">
        <i class="icon cart"></i>
        <span class="text">购物车<i class="cart-count">{{this.$store.state.cartList.length}}</i></span>
      </div>
    </div>
    <div class="bar-item bar-right">
      <div class="detail-menu">
        <div class="cart cart-and-buy" @click="addToCart">加入购物车</div>
        <div class="buy cart-and-buy" @click="buyClick">点击购买</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "DetailBottomBar",
  data() {
    return {
      isCollect: false
    }
  },
  methods: {
    addToCart() {
      this.$emit("addToCart");
    },
    collectClick() {
      return this.isCollect ? '已收藏' : '收藏'
    },
    cartClick() {
      this.$router.push('/cart')
    },
    buyClick() {
      this.$emit("addToCart");
      this.$router.push('/cart')
    }
  },
};
</script>

<style scoped>
.bottom-bar {
  display: flex;
  height: 49px;
  position: fixed;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #fff;
  text-align: center;
  box-shadow: 0px 1px 5px rgba(0, 0, 0, 0.3);
}

.bar-item {
  flex: 1;
  display: flex;
}

.bar-item > div {
  flex: 1;
}

.bar-left .text {
  font-size: 13px;
}

.bar-left .icon {
  display: block;
  width: 22px;
  height: 22px;
  margin: 5px auto 0px;
  background: url("~assets/img/detail/detail_bottom.png") 0 0/100%;
  background-size: 100%;
}

.bar-left .service {
  background-position: 0 -54px;
}

.bar-left .shop {
  background-position: 0 -98px;
}

.bar-left .cart {
  background: url("~assets/img/detail/cart.png") 0 0/100%;
}

.bar-left .collectActive {
  background: url("~assets/img/detail/detail_bottom.png") 0 0/100%;
  background-position: 0 -25px;
}

.bar-left .textActive {
  color: var(--color-high-text);
}

.cart-count {
  position: relative;
  top: -45px;
  right: -10px;
  background-color: var(--color-high-text);
  display: inline-block;
  width: 15px;
  height: 15px;
  border-radius: 50%;
  color: #fff;
  text-align: center;
  line-height: 15px;
}

.bar-right {
  display: flex;
  justify-content: center;
  align-items: center;
}
.detail-menu {
  display: flex;
  justify-content: center;
  align-items: center;
}
.cart-and-buy {
  height: 40px;
  width: 88px;
  line-height: 40px;
  font-size: 14px;
  color: white;
  font-weight: 500;
}
.cart {
  border-top-left-radius: 40px;
  border-bottom-left-radius: 40px;
  /* background-color: yellow; */
  background-image: linear-gradient(to right, #FFC500, #FF9402);
}
.buy {
  border-top-right-radius: 40px;
  border-bottom-right-radius: 40px;
  /* background-color: pink; */
  background-image: linear-gradient(to right, #FF7A00, #FE560A);
}
</style>