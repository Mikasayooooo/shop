<template>
  <div>
    
        <van-stepper   v-model="value" input-width="80px" button-size="20px" 
       ref="numbox" min="1" async-change  @change="countChange"/>
    
  </div>
</template>


<script>
export default {
    data(){
        return{
            value:this.initCount
        }
    },
    created(){
        // this.getinitCounts()
    },
    methods:{
    //    getinitCounts(){
    //        console.log(this.initCount);
           
    //    },
       countChange(){
        //    console.log(this.$refs.numbox.value);
// 数量改变了
      // console.log(this.$refs.numbox.value);
      // 每当数量值改变，则立即把最新的数量同步到 购物车的  store 中，覆盖之前的数量值
        this.$store.commit("updateCate",{
            id:this.goodsId,
            count:this.$refs.numbox.value
        })
           
       }
    },
 props:["initCount","goodsId"]
};
</script>

<style lang="scss" scoped>
</style>
