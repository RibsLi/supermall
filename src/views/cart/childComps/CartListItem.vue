<template>
  <div id="shopping-cart-list-item">
    <div class="item-selector">
      <check-button
        :is-checked="product.checked"
        @click.native="checkButtonClick"
      />
    </div>

    <div class="item-img">
      <img :src="product.img" alt="商品图片" />
    </div>

    <div class="item-info">
      <div class="item-name">
        <img src="~assets/img/profile/vip.svg" height="48" width="48" />
        {{ product.shopName }}
      </div>

      <div class="item-msg">{{ product.title }}</div>
      <div class="item-desc">{{ product.desc }}</div>

      <div class="info-bottom">
        <div class="item-price left">¥{{ product.price }}</div>

        <div class="item-count right">
          <div @click="decrement">－</div>
          <div>{{ product.count }}</div>
          <div @click="increment">＋</div>
        </div>
        <!-- <div v-show="!operation" class="bottom-delete" @click="deleteShop">
          删除
        </div> -->
      </div>
    </div>
  </div>
</template>

<script>
import CheckButton from "components/content/checkbutton/CheckButton";

export default {
  name: "CartListItem",
  components: {
    CheckButton,
  },
  props: {
    product: {
      type: Object,
      default() {
        return {};
      },
    },
  },

  methods: {
    checkButtonClick() {
      this.product.checked = !this.product.checked;
    },
    decrement() {
      if (this.product.count > 1) {
        this.product.count--;
      }
    },
    increment() {
      this.product.count++;
    },
    deleteShop() {
      this.$store.state.cartList.delete();
    },
  },

  computed: {
    price() {
      return (this.product.price * this.product.count).toFixed(2);
    },
  },
};
</script>

<style scoped>
#shopping-cart-list-item {
  width: 100%;
  display: flex;
  font-size: 0;
  padding: 10px;
  border-bottom: 1px solid #ccc;
}

.item-selector {
  width: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.item-name img {
  width: 20px;
  height: 20px;
  vertical-align: text-bottom;
}

.item-name,
.item-desc {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}

.item-desc {
  text-align: left;
}

.item-msg {
  text-align: left;
  color: var(--color-high-text);
  font-size: 13px;
  margin-top: 7px;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}

.item-img {
  padding: 5px;
}

.item-img img {
  width: 80px;
  height: 110px;
  display: block;
  border-radius: 5px;
}

.item-info {
  width: 100%;
  text-align: center;
  font-size: 17px;
  color: #333;
  padding: 5px 10px;
  position: relative;
  overflow: hidden;
}

.item-info .item-desc {
  font-size: 14px;
  color: #666;
  margin-top: 7px;
}

.info-bottom {
  margin-top: 7px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.info-bottom .item-price {
  color: orangered;
  margin-top: 13px;
}

.item-count {
  margin: 10px 18px 0 0;
  display: flex;
  text-align: center;
  height: 25px;
  width: auto;
}
.item-count div {
  height: 23px;
  line-height: 23px;
  outline: none;
  background-color: #fff;
  border: none;
  border: 1px solid rgba(0, 0, 0, 0.7);
  border-collapse: collapse;
  width: 30px;
}
.item-count div:last-child {
  border-bottom-right-radius: 5px;
  border-top-right-radius: 5px;
}
.item-count div:first-child {
  border-bottom-left-radius: 5px;
  border-top-left-radius: 5px;
}
.item-count div:nth-child(2) {
  border-right: none;
  border-left: none;
}
/* .bottom-delete {
    color: #fc0a0a;
    border: 1px solid #fc0a0a;
    font-size: 14px;
    padding: 4px 15px;
    text-align: center;
    border-radius: 80px;
    margin: 9px 18px 0 0;
  } */
</style>
