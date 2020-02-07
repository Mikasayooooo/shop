<template>
  <div>
    <!-- <div id="slider" class="mui-slider ">
				<div id="sliderSegmentedControl" class="mui-scroll-wrapper mui-slider-indicator mui-segmented-control mui-segmented-control-inverted">
					<div class="mui-scroll">
						<a class="mui-control-item mui-active" href="#item1mobile" data-wid="tab-top-subpage-1.html">
							推荐
						</a>
						<a class="mui-control-item" href="#item2mobile" data-wid="tab-top-subpage-2.html">
							热点
						</a>
						<a class="mui-control-item" href="#item3mobile" data-wid="tab-top-subpage-3.html">
							北京
						</a>
						<a class="mui-control-item" href="#item4mobile" data-wid="tab-top-subpage-4.html">
							社会
						</a>
						<a class="mui-control-item" href="#item5mobile" data-wid="tab-top-subpage-5.html">
							娱乐
						</a>
						<a class="mui-control-item" href="#item6mobile" data-wid="tab-top-subpage-6.html">
							科技
						</a>
					</div>
				</div>

    </div>-->

   <!-- 顶部滑动条区域 -->
    <van-tabs title-active-color="lightblue" background="pink" >
      <van-tab v-for="item in categoryList" :key="item.id" >

		  <div slot="title" @click="getImageList(item.id)">
			  {{item.title}}
		  </div>

	  </van-tab>
    </van-tabs>


	<!-- <div>

		<img v-for="img in imageList" v-lazy="img.img_url" :key="img.id">

	<div>{{img.title}}</div>
	</div> -->
	
	 <!-- 图片列表区域 -->
	<ul class="photo-list">
		<router-link v-for="img in imageList" :key="img.id" 
		:to="'/home/photoinfo/'+img.id" tag="li">
			<img v-lazy="img.img_url">
			<div class="info">
				<h1 class="info-title">  {{img.title}}</h1>
				<div class="info-body">{{img.zhaiyao}}</div>
			</div>
			
		</router-link>
	</ul>
  </div>
</template>

<script>
// import  mui from '../../lib/mui/js/mui.min.js'


export default {
  data() {
	return {
		// 所有分类的列表数组
		categoryList:[],
	// 图片列表的数组
	  imageList: [],
	//   item:[]

	};
	
  },
  created() {
	  this.getAllCategory();
	  this.getImageList(0);
  },
  mounted() {
    // mui('.mui-scroll-wrapper').scroll({
    //     deceleration: 0.0005
    //     //flick 减速系数，系数越大，滚动速度越慢，滚动距离越小，默认值0.0006
    // });
  },
  methods: {
	    // 获取所有的图片分类
	 getAllCategory() {
		 this.$http.get('http://www.liulongbin.top:3005/api/getimgcategory').then(result => {
			 if(result.body.status === 0) {
				   // 手动拼接出一个最完整的 分类列表
				 result.body.message.unshift( { title:"家居生活", id:0 });
				 this.categoryList = result.body.message;

				// this.categoryList.forEach((ele,i) => {
				// 	// console.log(ele);
					
				// 	this.item = ele;
				// 	console.log(this.item);
					
				// })

				// console.log('---------------');
				
				// console.log(this.item);
			 }
		 })
	 },

 // 根据 分类Id，获取图片列表
	 getImageList(pid) {
		 this.$http.get('http://www.liulongbin.top:3005/api/getimages/'+pid).then(result => {
			 if(result.body.status === 0) {
		
				 this.imageList = result.body.message;
				//  console.log(pid);
			 } 
		 })
	 }

	 
  }
};
</script>

<style lang="scss" scoped>
//  * {
// touch-action: pan-y;
// }

// .van-tab--active{
// 	color: pink!important;
// }

.photo-list{
	list-style: none;
	margin: 0;
	padding: 10px;
	padding-bottom: 0;
	li{
		position: relative;
		background-color:#ccc;
		text-align: center;
		margin-bottom: 10px;
		box-shadow: 0 0 10px #999;

		img{
			width: 100%;
			vertical-align: middle;
		}

		img[lazy="loading"]{
			width: 40px;
			height: 300px;
			margin: auto;
		}
	}
}

.info{
	position: absolute;
	color: white;
	bottom: 0px;
	text-align: left;
	background-color: rgba(0,0,0,0.4);
	max-height: 85px;

	.info-title{
		font-size: 15px;
	}
	.info-body{
		font-size: 13px;
	}
}
</style>