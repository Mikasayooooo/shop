<template>
    <div class="photoinfo-container">
        <h3 class="title">{{photoinfo.title}}</h3>
        <p class="subtitle">
            <span>发表时间：{{photoinfo.add_time | dateformat}}</span>
            <span>点击：{{photoinfo.click}}次</span>
        </p>

        <hr>
    <!-- 缩略图区域 -->
        <div class="thumbs">
            <vue-preview :slides="slide1" @close="handleClose"></vue-preview>
        </div>
   <!-- 图片内容区域 -->
        <div class="content" v-html="photoinfo.content"></div>
   <!-- 放置一个现成的 评论子组件 -->
        <comment :cid="id"></comment>
    </div>
</template>

<script>
// 1. 导入评论子组件
import comment from '../subcomponents/comment.vue'

export default {
    data(){
        return{
            // 从路由中获取到的 图片Id
            id:this.$route.params.id,
            // 图片详情
            photoinfo:{},
             // 缩略图的数组
            slide1: []
        }
    },
    created(){
        this.getPhotoInfo()
        this.getThumbs()
    },
    methods:{
         // 获取图片的详情
        getPhotoInfo(){
            this.$http.get('http://www.liulongbin.top:3005/api/getimageInfo/'+this.id)
            .then(result => {
                if(result.body.status === 0) {
                    this.photoinfo = result.body.message[0]
                } 
            })
        },

// 获取缩略图
        getThumbs(){
            this.$http.get('http://www.liulongbin.top:3005/api/getthumimages/'+this.id)
            .then(result => {
                if(result.body.status === 0) {
                    // 循环每个图片数据，补全图片的宽和高
                    result.body.message.forEach(item => {
                        item.w = 600;
                        item.H = 400;
                        item.alt='picture'
                        item.title='图片'
                        item.msrc = item.src
                    });
                    // 把完整的数据保存到 slide1 中
                    this.slide1 = result.body.message
                } 
            })
        },
        handleClose(){
            console.log('close event');
            
        }
    },
    components:{
        // 注册 评论子组件
        comment
    }
}
</script>

<style lang="scss" scoped>
    .photoinfo-container{
        padding: 3px;
        .title{
             font-size: 15px;
            color: #226aff;
            text-align: center;
            margin: 15px 0;
        }
        .subtitle{
            font-size: 13px;
            display: flex;
            justify-content: space-between;
        }
        .content{
            font-size: 13px;
            line-height: 30px;
        }
    }
   
     .thumbs {
    /deep/ .my-gallery {
        //deep深层作用选择器
        display: flex;
        //默认换行
        flex-wrap: wrap;
        figure {
            width: 30%;
            margin: 5px;
            img {
                width: 100%;
                box-shadow: 0 0 8px #999;
                border-radius: 5px;
            }
        }
    }
}
    
</style>