<template>
  <div class="normal-list">
    <div v-for="(item, index) in serviceList" :key="index" class="item">
      <span class="icon">
        <img :src="iconURL(item)" alt="" />
      </span>
      <div class="info">{{ item.info }}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "NormalListView",
  data() {
    return {
      serviceList: [
        { icon: "message.svg", info: "消息" },
        { icon: "pointer.svg", info: "积分" },
        { icon: "vip.svg", info: "VIP会员专属" },
        { icon: "cart.svg", info: "我的购物车" },
        { icon: "shopping.svg", info: "下载购物APP" },
        { icon: "settings.svg", info: "设置" },
      ],
    };
  },
  methods: {
    iconURL(item) {
      return require("assets/img/profile/" + item.icon);
    },
  },
};
</script>

<style scoped>
.normal-list {
  background-color: #fff;
  font-size: 15px;
  color: #333;
  margin-top: 15px;
}
.normal-list .item {
  height: 44px;
  line-height: 44px;
  position: relative;
  border-bottom: 1px solid rgba(100, 100, 100, 0.1);
}
.icon img {
  width: 20px;
  height: 20px;
}
.item .icon {
  margin-top: 10px;
  position: absolute;
  left: 16px;
  top: -7px;
}
.item .info {
  margin-left: 40px;
  padding-left: 5px;
}
</style>