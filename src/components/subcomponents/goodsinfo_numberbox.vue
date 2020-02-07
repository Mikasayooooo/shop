<template>
  <div>
    
        <van-stepper v-model="value" input-width="80px" button-size="40px" 
        @change="getCount" ref = "numbox" min="1" :max="max" async-change/>
    
  </div>
</template>


<script>
export default {
    data(){
        return{
            value:1
        }
    },
    methods:{
        getCount(){
              // 每当 文本框的数据被修改的时候，立即把 最新的数据，通过事件调用，传递给父组件
            console.log(this.$refs.numbox.value);
            this.$emit('getCount',parseInt(this.$refs.numbox.value));
            
        }
    },
    props:["max"]
};
</script>

<style lang="scss" scoped>
</style>
