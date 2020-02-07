<template>
  <div>
    <!-- 轮播图区域 -->
    <swipe :lunbotuList="lunbotuList" :isfull="true"></swipe>
    <!-- 九宫格 到 6宫格 的改造工程 -->
    <ul class="mui-table-view mui-grid-view mui-grid-9">
      <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-3">
        <router-link to="/home/newslist">
          <img src="../../images/menu1.png" alt />
          <div class="mui-media-body">新闻资讯</div>
        </router-link>
      </li>
      <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-3">
        <router-link to="/home/photolist">
          <img src="../../images/menu2.png" alt />
          <div class="mui-media-body">图片分享</div>
        </router-link>
      </li>
      <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-3">
        <router-link to="/home/goodslist">
          <img src="../../images/menu3.png" alt />
          <div class="mui-media-body">商品购买</div>
        </router-link>
      </li>
      <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-3">
        <a href="#">
          <img src="../../images/menu4.png" alt />
          <div class="mui-media-body">留言反馈</div>
        </a>
      </li>
      <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-3">
        <a href="#">
          <img src="../../images/menu5.png" alt />
          <div class="mui-media-body">视频专区</div>
        </a>
      </li>
      <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-3">
        <a href="#">
          <img src="../../images/menu6.png" alt />
          <div class="mui-media-body">联系我们</div>
        </a>
      </li>
    </ul>
  </div>
</template>

<script>
import { Toast } from "mint-ui";
import swipe from "../subcomponents/swipe.vue";

export default {
  data() {
    return {
      // 保存轮播图的数组
      lunbotuList: []
    };
  },
  created() {
    this.getLunbotu();
  },
  methods: {
    // 获取轮播图数据的方法
    getLunbotu() {
      this.$http
        .get("https://api.zbztb.cn/api/public/v1/home/swiperdata")
        .then(result => {
          console.log(result);
          // 成功了
          if (result.body.meta.status === 200) {
            this.lunbotuList = result.body.message;
          } else {
            // 失败的
            Toast("加载失败了。。");
          }
        });
    }
  },

  components: {
    swipe
  }
};
</script>

<style lang="scss" scoped>
.mui-grid-view.mui-grid-9 {
  background-color: #fff;
  border: none;
}

// ul{
//   background-color: #fff;
//     border: none;
// }
.mui-grid-view.mui-grid-9 .mui-media {
  border: 0;
  img {
    width: 50%;
    height: 50%;
  }

  .mui-media-body {
    font-size: 50%;
  }
}
</style>