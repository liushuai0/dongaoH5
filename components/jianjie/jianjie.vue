<template>
	<view>
		<view class="card">
			<!-- <image src="../../static/image/jianjie.png" :style="{height:height+'rpx'}" class="jjImg" mode=""></image> -->
<!-- 			<view class="card_title">2022年北京冬奥会</view> -->
		<!-- <rich-text :nodes="description"></rich-text> -->
				<!-- <image src="../../static/image/bglong.jpg" class="content_img" style=""></image> -->
				
				 <image src="../../static/image/001.jpg" class="content_img" mode="widthFix"></image>
				<image src="../../static/image/002.jpg" class="content_img" mode="widthFix"></image>
				 <image src="../../static/image/003.jpg" class="content_img" mode="widthFix"></image>
				<image src="../../static/image/004.jpg" class="content_img" mode="widthFix"></image>
				<image src="../../static/image/005.jpg" class="content_img" mode="widthFix"></image>
				<image src="../../static/image/006.jpg" class="content_img" mode="widthFix"></image> 
				
		</view>
	</view>
</template>

<script>
	import http from "../../common/getList.js"
	export default {

		data() {
			return {
				height:6300,
				
			}
		},
		mounted(){
			// let query = uni.createSelectorQuery();
			//  var height
			//  query.select('.card').boundingClientRect(rect=>{
			// 	 console.log(rect);
			//    let clientHeight = rect.height;
			//    let clientWidth = rect.width;
			//    let ratio = 750 / clientWidth;
			//    height = clientHeight * ratio;
			//    console.log(height);
			//    this.height=height+10
			//  }).exec();
0
		},
		methods: {
			// getList(e){
			// 	let url="api/home/index/index"
			// 	let data={
					
			// 	}
			// 	http.request(url,data).then(
			// 	res=>{
			// 		console.log(res);
			// 		if(res.code===1){
			// 			this.description=res.data.description
					
			// 		}
			// 	},
			// 	error=>{
			// 		console.log(error);
			// 	}
			// 	)
			// }
		}
	}
</script>

<style>
.card{
	display: flex;
	flex-direction: column;
	align-items: center;
	color: #7489B2;
	padding: 60rpx 60rpx 0 60rpx;
	
/* 	background-image: url('../../static/image/jianjie.png');
	background-repeat: no-repeat;
	background-size: 700rpx;
	background-position: center; */
}
.card_title{
	font-size: 34rpx;
	margin-bottom: 40rpx;
	font-weight: 600;
	margin-top: 70rpx;
}
.card_content{
	font-size: 30rpx;
}
.jjImg{
	position: absolute;
	z-index: -1;
	width: 700rpx;
}
.content_img{
	width: 700rpx;
	height: 1112rpx;
}
</style>
