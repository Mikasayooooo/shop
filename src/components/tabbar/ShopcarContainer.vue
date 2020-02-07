<template>
  <div class="goods-container">
    
    <div class="goods-list">
        <!-- 商品列表项区域 -->
      <div class="mui-card" v-for="(item,i) in goodslist" :key="item.id">
        <div class="mui-card-content">
          <div class="mui-card-content-inner">
            <mt-switch  v-model="$store.getters.getGoodsSelected[item.id]"
            @change="selectedChange(item.id,$store.getters.getGoodsSelected[item.id])"
            ></mt-switch>
           <!-- :value="checked" @input="onInput(item.id,$store.getters.getGoodsSelected[item.id])" -->
            <img :src="item.thumb_path" />

            <div class="info">
              <h1>{{item.title}}</h1>
              <p>
                <span class="price">￥{{item.sell_price}}</span>
                <numberbox :initCount="$store.getters.getGoodsCount[item.id]"
                :goodsId ="item.id"></numberbox>
                <!-- 问题：如何从购物车中获取商品的数量呢 -->
                <!-- 1. 我们可以先创建一个 空对象，然后循环购物车中所有商品的数据， 把 当前循环这条商品的 Id， 作为 对象 的 属性名，count值作为 对象的 属性值，这样，当把所有的商品循环一遍，就会得到一个对象： { 88: 2, 89: 1, 90: 4 } -->
                <a href="#" @click.prevent="remove(item.id,i)">删除 </a>
              </p>
            </div>
          </div>
        </div>
      </div>
    </div>

<!-- 结算区域 -->
    <div class="mui-card">
      
      <div class="mui-card-content">
        <div class="mui-card-content-inner jiesuan" >
            <div class="left">
                <p>总计(不含运费)</p>
                <p>已勾选<span class="red">{{$store.getters.getGoodsCountAndAmount.count}}</span>件,
                总价￥<span class="red">{{$store.getters.getGoodsCountAndAmount.amount}}</span>元</p>
            </div>
               <mt-button type="danger" >去结算</mt-button>
        </div>
 
        
      </div>
           
      
    </div>
  </div>
</template>

<script>
import numberbox from "../subcomponents/shopcar_numberbox.vue";

export default {
  data() {
    return {
    //   checked: this.$store.getters.getGoodsSelected[item.id],
    // checked:true,
    // 购物车中所有商品的数据
      goodslist:[]
    };
  },
  created(){
      this.getGoodsList()
  },
  methods:{
      getGoodsList(){
// 1. 获取到 store 中所有的商品的Id，然后拼接出一个 用逗号分隔的 字符串
          var idArr = [];
          this.$store.state.cart.forEach(item => {
              idArr.push(item.id);
          });
            // 如果 购物车中没有商品，则直接返回，不需要请求数据接口，否则会报错
          if(idArr.length<=0){
            return;
          }

          this.$http.get('http://www.liulongbin.top:3005/api/goods/getshopcarlist/'
          +idArr.join(','))
          .then(result =>{
                 if(result.body.status === 0) {
                    this.goodslist = result.body.message;
                } 
            })
      },

// 点击删除，把商品从 store 中根据 传递的 Id 删除，同时，把 当前组件中的 goodslist 中，对应要删除的那个商品，使用 index 来删除
      remove(id,index){
          this.goodslist.splice(index,1);
          
          this.$store.commit("removeCart",id);
      },

      selectedChange(id,val){
          // 每当点击开关，把最新的 快关状态，同步到 store 中
        //   console.log(checked);
          this.$store.commit("updateSelectedChange",{id,selected:val})
      },
   
  },
  components: {
    numberbox
  }
};
</script>

<style lang="scss" scoped>
.goods-container {
  background: #eee;
  overflow: hidden;

  .goods-list {
    .mui-card-content-inner {
      display: flex;
      align-items: center;
    }

    img {
      width: 80px;
      height: 80px;
      margin: 10px;
    }

    .info {
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      padding: 4px;

      h1 {
        font-size: 13px;
      }
      .price {
        color: red;
        font-weight: bold;
      }
    }
  }

  .jiesuan{
      display: flex;
      justify-content: space-between;
      .red{
          color: red;
          font-size: 16px;
          font-weight: bold;
      }
  }
}


</style>