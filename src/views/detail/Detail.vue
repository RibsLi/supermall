<template>
  <div class="detail">
    <detail-nav-bar class="detail-nav" @titleClick="titleClick" ref="navBar" />
    <scroll
      class="content"
      ref="scroll"
      :pull-up-load="true"
      @scroll="contentScroll"
      :probe-type="3"
    >
      <detail-swiper :top-img="topImg" />
      <detail-base-info :goods="goods" />
      <detail-shop-info :shop="shop" />
      <detail-param-info :param-info="paramInfo" ref="params" />
      <detail-goods-info
        :detail-info="detailInfo"
        @detailImagLoad="detailImagLoad"
      />
      <detail-comment-info :comment-info="commentInfo" ref="comment" />
      <goods-list :goods="recommends" ref="recommend" :isRecommend="true" />
      <detail-base-line />
    </scroll>
    <detail-bottom-bar @addToCart="addToCart" />
    <back-top @click.native="backTopClick" v-show="isShowBackTop" />
    <!-- <toast :message="message"/> -->
    <div class="toast" v-show="isShow">加入购物车成功</div>
  </div>
</template>

<script>
import DetailNavBar from "./childComps/DetailNavBar";
import DetailSwiper from "./childComps/DetailSwiper";
import DetailBaseInfo from "./childComps/DetailBaseInfo";
import DetailShopInfo from "./childComps/DetailShopInfo";
import Scroll from "components/common/scroll/Scroll";
import DetailGoodsInfo from "./childComps/DetailGoodsInfo";
import DetailParamInfo from "./childComps/DetailParamInfo";
import DetailCommentInfo from "./childComps/DetailCommentInfo";
import GoodsList from "components/content/goods/GoodsList";
import DetailBottomBar from "./childComps/DetailBottomBar";
// import BackTop from 'components/content/backTop/BackTop'
import DetailBaseLine from "./childComps/DetailBaseLine";
// import Toast from 'components/common/toast/Toast'

import {
  getDetail,
  Goods,
  Shop,
  GoodsParam,
  getRecommend,
} from "network/detail";
import { debounce } from "common/utils";
import { backTopMixin } from "common/mixin";

export default {
  name: "Detail",
  components: {
    DetailNavBar,
    DetailSwiper,
    DetailBaseInfo,
    DetailShopInfo,
    Scroll,
    DetailGoodsInfo,
    DetailParamInfo,
    DetailCommentInfo,
    GoodsList,
    DetailBottomBar,
    DetailBaseLine,
    // Toast,
    // BackTop
  },
  mixins: [backTopMixin],
  data() {
    return {
      iid: null,
      topImg: [],
      goods: {},
      shop: {},
      detailInfo: {},
      paramInfo: {},
      commentInfo: {},
      recommends: [],
      themeTopYs: [],
      getThemeTopYs: null,
      currentIndex: 0,
      isShowBackTop: false,
      message: "",
      isShow: false,
    };
  },

  created() {
    //保存传入的iid,此处因为keep-alive会一直保存iid需要在keep-alive里面排除Detail组件
    // this.iid = this.$route.params.iid
    this.iid = this.$route.query.iid;
    //根据iid获取数据
    getDetail(this.iid).then((res) => {
      // console.log(res);
      const data = res.data.result;
      this.topImg = data.itemInfo.topImages;
      //获取商品信息
      this.goods = new Goods(data.itemInfo, data.columns, data.shopInfo);
      //商家信息
      this.shop = new Shop(data.shopInfo);
      //保存商品详情数据
      this.detailInfo = data.detailInfo;
      //商品参数
      this.paramInfo = new GoodsParam(
        data.itemParams.info,
        data.itemParams.rule
      );
      //评论信息
      if (data.rate.cRate !== 0) {
        this.commentInfo = data.rate.list[0];
      }
    });
    // 详情页推荐商品
    getRecommend().then((res) => {
      // console.log(res);
      this.recommends = res.data.data.list;
    }),
      // 详情页防抖
      (this.getThemeTopYs = debounce(() => {
        // console.log('---');
        this.themeTopYs = [];
        this.themeTopYs.push(0);
        this.themeTopYs.push(this.$refs.params.$el.offsetTop);
        this.themeTopYs.push(this.$refs.comment.$el.offsetTop);
        this.themeTopYs.push(this.$refs.recommend.$el.offsetTop);
        this.themeTopYs.push(Number.MAX_VALUE);
        // console.log(this.themeTopYs);
      }));
  },

  methods: {
    titleClick(index) {
      this.$refs.scroll.scrollTo(0, -this.themeTopYs[index], 200);
    },
    //监听图片加载完成
    detailImagLoad() {
      //详情页导航点击事件
      this.getThemeTopYs();
    },
    // 监听scroll完成滚动详情页跳转导航
    contentScroll(createpositon) {
      // console.log(position);
      const positionY = -createpositon.y;
      const length = this.themeTopYs.length;
      for (let i = 0; i < length - 1; i++) {
        if (
          this.currentIndex !== i &&
          positionY >= this.themeTopYs[i] &&
          positionY < this.themeTopYs[i + 1]
        ) {
          this.currentIndex = i;
          this.$refs.navBar.currentIndex = this.currentIndex;
          // 解决currentindex报错问题
          this.currentIndex = 0;
        }

        // // 条件一防止频繁赋值，条件二判断i在0和length-1之间，条件三判断i大于length-1
        // if(this.currentIndex !== i && (i < length - 1 && positionY >= this.themeTopYs[i] && positionY < this.themeTopYs[i + 1]) || (i === length - 1 && positionY >= this.themeTopYs[i])) {
        //   this.currentIndex = i
        //   // console.log(this.currentIndex);
        //   this.$refs.navBar.currentIndex = this.currentIndex
        // }
      }
      this.isShowBackTop = -createpositon.y > 800;
    },
    // 返回顶部
    backTopClick() {
      this.$refs.scroll.scrollTo(0, 0);
    },
    //加入购物车
    addToCart() {
      const product = {};
      product.img = this.topImg[0];
      product.title = this.goods.title;
      product.desc = this.goods.desc;
      product.price = this.goods.realPrice;
      product.iid = this.iid;
      product.shopName = this.shop.name;
      product.count = 1;

      this.$store.dispatch("addCart", product).then((res) => {
        // console.log(res);
        // this.message = res
        // this.$toast.show(res, 1000)
      });
      this.isShow = true;
      setTimeout(() => {
        this.isShow = false;
      }, 1000);
    },
  },
};
</script>

<style scoped>
.detail {
  position: relative;
  z-index: 9;
  background-color: #fff;
  height: 100vh;
}
.detail-nav {
  position: relative;
  z-index: 9;
  background-color: #fff;
}
.content {
  position: absolute;
  top: 44px;
  bottom: 49px;
  left: 0;
  right: 0;
  overflow: hidden;
}
.toast {
  position: fixed;
  z-index: 999999;
  left: 50%;
  top: 45%;
  transition: all 0.5s;
  -webkit-transform: translateX(-50%) translateY(-50%);
  -moz-transform: translateX(-50%) translateY(-50%);
  -ms-transform: translateX(-50%) translateY(-50%);
  -o-transform: translateX(-50%) translateY(-50%);
  transform: translateX(-50%) translateY(-50%);
  text-align: center;
  border-radius: 8px;
  color: #fff;
  background: rgba(17, 17, 17, 0.5);
  height: 50px;
  line-height: 50px;
  padding: 0 15px;
  max-width: 200px;
}
</style>