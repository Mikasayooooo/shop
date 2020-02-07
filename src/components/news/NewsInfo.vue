<template>
    <div>
        <div class="newsinfo_container">
             <!-- 大标题 -->
            <h3 class="title">{{newsinfo.title}}</h3>
              <!-- 子标题 -->
           <p class="subtitle">
            <span>发表时间：{{newsinfo.add_time  | dateformat}}</span>
            <span>点击:{{newsinfo.click}}次</span>
           </p>
        </div>

        <hr>
 <!-- 内容区域 -->
        <div class="content" v-html="newsinfo.content"></div>
 <!-- 评论子组件区域 -->
        <comment-box :cid="id"></comment-box>
    </div>
</template>

<script>

    // 1. 导入 评论子组件
    import comment from '../subcomponents/comment.vue'

export default {
    data(){
        return{
            // 将 URL 地址中传递过来的 Id值，挂载到 data上，方便以后调用
            id:this.$route.params.id,
             // 新闻对象
            newsinfo:{}
        }
    },
    created(){
        this.getNewsInfo()
    },
    methods:{
        getNewsInfo(){
             // 获取新闻详情
            this.$http.get('http://www.liulongbin.top:3005/api/getnew/'+this.id).then(result => {
                // console.log(result);
                
                if(result.body.status === 0) {
                    this.newsinfo = result.body.message[0]
                }else {
                    Toast('加载失败了。。')
                }
            })
        }
    },
    components:{
        // 用来注册子组件的节点
        "comment-box":comment
    }
 
}
</script>

<style lang="scss" >
        .newsinfo_container{
            padding: 0 4px;
            .title{
                font-size: 16px;
                text-align: center;
                color: red;
                margin: 15px 0;
            }
            .subtitle{
                font-size: 13px;
                color: #226aff;
                display: flex;
                justify-content: space-between;
            }
        }
        .content{
            img {
                width: 100%;
            }
        }
</style>