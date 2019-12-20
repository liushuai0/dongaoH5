<template>
	<view class="Containe1r">
		<view class="searchView">
			<input type="text" class="searchtext" v-model="searchVal" placeholder="请输入搜索内容" />
			<button type="primary" class="searchBtn" @tap="search1">搜 索</button>
		</view>
		<block v-if="!isSearch" v-for="(item, index) in imgurl" :key="index">
			<view class="">
				<image :src="item.pic_url1" class="img1" :data-index="index" @tap="privewImg"></image>
				<view class="" style="text-align: center;font-size: 30rpx;margin-top: 10rpx;">{{ item.name }}-{{ item.num }}号</view>
			</view>
		</block>
		<block v-if="isSearch" v-for="(item, index) in searchList" :key="index">
			<view class="">
				<image :src="item.pic_url1" class="img1" :data-index="index" @tap="privewImg"></image>
				<view class="" style="text-align: center;font-size: 30rpx;margin-top: 10rpx;">{{ item.name }}-{{ item.num }}号</view>
			</view>
		</block>
	</view>
</template>

<script>
import http from '../../common/getList.js';
export default {
	props: {
		imgurl: {}
	},
	data() {
		return {
			page: 0,
			cpage: 1,
			searchVal: '',
			isSearch: false,
			searchList: []
		};
	},
	mounted() {
		// this.getList()
	},
	methods: {
		// getList(e) {
		// 	var that = this;
		// 	let url = 'api/home/index/videoList';
		// 	let data = {
		// 		page: that.page
		// 	};
		// 	http.request(url, data).then(
		// 		res => {
		// 			console.log(res);
		// 			if (res.code === 1) {
		// 				that.searchList = res.data.list;
		// 			}
		// 		},
		// 		error => {}
		// 	);
		// },
		search1(e) {
			var that = this;
			if (that.searchVal != null && that.searchVal) {
				let url = 'api/home/index/search';
				let data = {
					keywords: that.searchVal
				};
				http.request(url, data).then(
					res => {
						if (res.code === 1) {
							that.isSearch = true;
							that.searchList = res.data.list;
						}
					},
					error => {}
				);
			} else {
				that.isSearch = false;
			}
		},
		privewImg(e) {
			var that = this;
			let index = e.currentTarget.dataset.index;
			var imgobj = [];
			if (that.isSearch == true) {
				imgobj.push(that.searchList[index].pic_url1);
				imgobj.push(that.searchList[index].pic_url2);
				
				uni.previewImage({
					urls: imgobj
				});
			} else {
				imgobj.push(that.imgurl[index].pic_url1);
				imgobj.push(that.imgurl[index].pic_url2);
				uni.previewImage({
					urls: imgobj
				});
			}
		}
	},

	components: {}
};
</script>

<style>
.Containe1r {
	margin-top: 20rpx;
	display: flex;
	flex-direction: row;

	flex-wrap: wrap;
	width: 730rpx;
	margin: 0 auto;
}
.img1 {
	width: 340rpx;
	margin-top: 20rpx;
	margin-left: 18rpx;
}
.searchView {
	width: 750rpx;
	padding: 0 30rpx;
	display: flex;
	flex-direction: row;
	justify-content: space-between;
	align-items: center;
	position: relative;
	margin-top: 20rpx;
}
.searchBtn {
	width: 210rpx;
	height: 60rpx;
	line-height: 60rpx;
	font-size: 30rpx;
	position: absolute;
	right: 30rpx;
}
.searchtext {
	border: 1rpx solid #007aff;
	height: 60rpx;
	line-height: 60rpx;
	padding-left: 15rpx;
	border-radius: 10rpx;
	width: 430rpx;

	font-size: 30rpx;
}
</style>
