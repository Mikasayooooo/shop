<template>
  <div class="goodsdesc-container">
    <div class="title">{{desc.title}}</div>
    <hr />
    <div class="content" v-html="desc.content"></div>
  </div>
</template>

<script>

export default {
  data() {
    return {
      // 图文数据
      desc: {}
    };
  },
  created() {
    this.getDesc();
  },
  methods: {
    getDesc() {
      this.$http
        .get(
          "http://www.liulongbin.top:3005/api/goods/getdesc/" +
            this.$route.params.id
        )
        .then(result => {
          console.log(result);
          if (result.body.status === 0) {
            this.desc = result.body.message[0];
          } 
        });
    }
  }
};
</script>

<style lang="scss"  >
.goodsdesc-container {
    padding: 4px;
  .title {
    font-size: 16px;
    color: #226aff;
    text-align: center;
    margin: 15px 0;
  }
  .content {
      img{
          width: 100%;
      }
  }
}
</style>