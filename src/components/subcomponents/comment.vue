<template>
    <div class="cmt-container">
        <h3>发表评论---{{this.cid}}</h3>
        <hr>
        <textarea placeholder="请输入你要BB的内容" maxlength="120" v-model="msg"></textarea>

        <mt-button type = "primary" size="large" @click="postMessage">发表评论</mt-button>

        <div class="cmt-list">
            <div class="cmt-item" v-for="(item, index) in commentslist" :key="index">
                <div class="cmt-title">
                   第{{index+1}}楼&nbsp;&nbsp;&nbsp;用户：{{item.user_name}} 发表时间：{{item.add_time}}
                </div>
                <div class="cmt-body">
                {{item.content === 'undefined' ? '此用户很难蓝，什么也没有留下' : item.content}} 
                </div>
            </div>
        </div>

        <mt-button type = "danger" size="large" plain @click="getMore">加载更多</mt-button>
    </div>
</template>

<script>
    import {Toast} from 'mint-ui'

export default {
    data(){
        return{
            // 默认展示第一页数据
            pageindex:1,
            // 所有的评论数据
            commentslist:[],
            // 评论输入的内容
            msg:''
        }
    }, 
    
    created(){
        this.getComments()
    },

    props:["cid"],
    
    methods:{

            // 获取评论
        getComments(){
            this.$http.get("http://www.liulongbin.top:3005/api/getcomments/"+this.cid+"?pageindex="+this.pageindex)
            // this.$http.get('api/getcomments/15?pageindex=1')
            .then(result => {
                if(result.body.status === 0) {
                    // this.commentslist = result.body.message
                    console.log(result.body);
    // 每当获取新评论数据的时候，不要把老数据清空覆盖，而是应该以老数据，拼接上新数据
                    this.commentslist = this.commentslist.concat(result.body.message)
                } else {
                    Toast('加载失败了。。。。')
                }
            })
        },

// 加载更多
        getMore(){
            this.pageindex++;
            this.getComments()
        },

        postMessage(){
  // 校验是否为空内容
            if(this.msg.trim().length === 0) {
                return Toast('评论内容不嫩为空')
            }

              // 发表评论
      // 参数1： 请求的URL地址
      // 参数2： 提交给服务器的数据对象 { content: this.msg }
      // 参数3： 定义提交时候，表单中数据的格式  { emulateJSON:true }

            this.$http.post('http://www.liulongbin.top:3005/api/postcomment/'+this.cid,{
                content:this.msg.trim()
            })
            .then(result => {
                if(result.body.status === 0) {
                    // 1. 拼接出一个评论对象
                    var cmt = {
                        user_name:'匿名用户',
                        add_time:Date.now(),
                        content: this.msg.trim()

                       
                    }
                     this.commentslist.unshift(cmt)
                        this.msg = ''
                } 
            })
        }
    }
    
   
    // props  Uncaught TypeError: Cannot read property 'type' of undefined
}
</script>

<style lang="scss" scoped>
    .cmt-container{
        h3{
            font-size: 18px;
        }
        textarea{
            font-size: 15px;
            height: 85px;
            margin: 0
        }
        .cmt-list{
            margin: 5px 0;
            .cmt-item{
                font-size: 14px;
                .cmt-title{
                    background: #ccc;
                    line-height: 30px;
                }
                .cmt-body{
                    line-height: 35px;
                    text-indent: 2em;
                }
            }
        }
    }
</style>