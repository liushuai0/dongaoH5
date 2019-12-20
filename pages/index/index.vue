<template>
	<view class="Container">
		<view class="top">
			<swiper :indicator-dots="true" class="swiper1" :autoplay="true" :interval="3000" :duration="1000">
				<swiper-item v-for="(item, index) in banner" :key="index">
					<view class="swiper-item">
						
						<image :src="item.image" @tap="tiaozhuan(item.url)"></image>
					</view>
				</swiper-item>
			</swiper>
		</view>
		<view class="header">
			<view class="coverCont" :class="currentPlay1 == index ? 'none1' : ' '">
				<image src="../../static/image/play1.png" @tap="playvideo1" class="playicon" />
				<text class="playtext">点击播放</text>
				<image src="../../static/image/001.png" class="fengmian" />
			</view>
			<video :src="c_video" class="img1" autoplay="true" v-if="currentPlay1 == index" @ended="playend1" :show-center-play-btn="false" controls></video>
		</view>
		<view class="header">
			<view class="coverCont" :class="currentPlay2 == index ? 'none1' : ' '">
				<image src="../../static/image/play1.png" @tap="playvideo2" class="playicon" />
				<text class="playtext">点击播放</text>
				<image src="../../static/image/002.png" class="fengmian" />
			</view>
			<video src="../../static/image/zhanshi1.mp4" class="img1" autoplay="true" @ended="playend2" v-if="currentPlay2 == index" :show-center-play-btn="false" controls></video>
		</view>

		<view class="content">
			<semp-notice-bar class="scrollTest" style="" scrollable showType="slider" strText="各位选手提交报名后,组委会将于每天18点进行审核通过。"></semp-notice-bar>
			<view class="nav">
				<view class="navBtn" :class="isshow === 1 ? 'on' : ''" @tap="show(1)">活动简介</view>
				<view class="navBtn" :class="isshow === 2? 'on' : ''" @tap="show(2)">在线报名</view>
				<view class="navBtn" :class="isshow === 3 ? 'on' : ''" @tap="show(3)">选手展示</view>
			</view>
		</view>
		<jianjie v-show="isshow === 1"></jianjie>
		
		<signUp v-if="isshow === 2"></signUp>
		<playershow v-show="isshow === 3" :imgurl="imgurl">

		</playershow>

		<text class="f_text" @tap="toAgam()">由阿甘派提供技术支持</text>
	</view>
</template>

<script>
import jianjie from '../../components/jianjie/jianjie.vue';
import llsignUp from '../../components/liaoyuanSignup/llsignUp.vue'
import playershow from '../../components/playershow/playershow.vue';
import signUp from '../../components/signUp/signUp.vue';
import http from '../../common/getList.js';
import uniNoticeBar from '../../components/uni-notice-bar/uni-notice-bar.vue';
import sempNoticeBar from '@/components/semp-notice-bar/semp-notice-bar.vue';
export default {
	data() {
		return {
			isshow: 1,
			banner: [],
			description: '',
			c_video: '',
			page: 0,
			pages: 0,
			title: '',
			videoList: [],
			currentPlay1: 0,
			currentPlay2: 0,
			index: 10,
			list: [
				'各位小朋友提交报名后，组委会将于每天18点进行审核通过',
				'各位小朋友提交报名后，组委会将于每天18点进行审核通过',
				'各位小朋友提交报名后，组委会将于每天18点进行审核通过'
			],
			imgurl: [] //图片数组
		};
	},
	onLoad(options) {
		
		this.getList();
	},

	components: {
		jianjie,
		llsignUp,
		playershow,
		signUp,
		uniNoticeBar,

		sempNoticeBar
	},
	methods: {
		toLiaoyuan(){
			this.isshow=4
		},
		toAgam(){
			parent.location.href="http://wx.agampai.com/";
			
		},
		show(index) {
			this.isshow = index;
			if (this.isshow === 3) {
				this.getVideo();
			}
		},
		tiaozhuan(url){
			if(url&&url!=null){
				parent.location.href=url;
			}
		},
		getList(e) {
			let url = 'api/home/index/index';
			let data = {};
			http.request(url, data).then(
				res => {
					if (res.code === 1) {

						this.description = res.data.description;
						this.banner = res.data.banner;
						this.c_video = res.data.video.video;
						this.title = res.data.video.title;
						uni.hideLoading();
					}
				},
				error => {
					uni.hideLoading();
				}
			);
		},
		getVideo(e) {
			var that = this;
			let url = 'api/home/index/videoList';
			let data = {
				page: that.page
			};
			http.request(url, data).then(
				res => {
					that.pages = res.data.totalpage;
					for (var i = 0; i < res.data.list.length; i++) {
						that.imgurl.push(res.data.list[i]);
					}
				},
				error => {}
			);
		},
		playvideo1(e) {
			this.currentPlay1 = this.index;
		},
		playvideo2(e) {
			this.currentPlay2 = this.index;
		},
		playend1(e) {
			this.currentPlay1 = 0;
		},
		playend2(e) {
			this.currentPlay2 = 0;
		}
	},

	onReachBottom(e) {
		var that = this;
		if (that.isshow === 3) {
			if (that.page < that.pages) {
				that.page++;
				that.getVideo();
			} else {
			}
		}
	}
};
</script>

<style>
.on {
	background-color: #007aff !important;
	color: white !important;
}
.none1 {
	display: none !important;
}
.fengmian {
	width: 655rpx;
	height: 370rpx;
}
.playicon {
	width: 60rpx;
	height: 60rpx;
	position: absolute;
	z-index: 999;
}
.playtext {
	position: absolute;
	z-index: 999;
	width: 300rpx;
	margin-top: 50rpx;
	margin-left: 90rpx;
	font-size: 28rpx;
	color: white;
}
.coverCont {
	display: flex;
	align-items: center;
	justify-content: center;
	position: relative;
	border-radius: 5rpx;
}
.scrollTest {
	font-size: 26rpx !important;
	margin-top: -70rpx;
}
.Container {
	display: flex;
	flex-direction: column;
	background-image: url('../../static/image/bg.png');
}
.top {
}
.img1 {
	width: 655rpx;
	height: 370rpx;
	margin: 0 auto;
}
.header {
	background-image: url('../../static/image/sBorder.png');
	background-repeat: no-repeat;
	background-size: 670rpx 390rpx;
	background-position: center;
	height: 390rpx;
	margin-top: 50rpx;
	display: flex;
	justify-content: center;
	align-items: center;
}
.swiper1 {
	width: 750rpx;
	height: 480rpx;
	margin: 0 auto;
	display: flex;
	justify-content: center;
	align-items: center;
}
.swiper-item image {
	width: 100%;
	height: 480rpx;
	margin-top: 6rpx;
}
.c_tip {
	width: 100%;
	font-size: 26rpx;
	text-align: center;
	color: #007aff;
	height: 150rpx;
	line-height: 150rpx;
	padding-top: -100rpx;
}
.nav {
	display: flex;
	flex-direction: row;
	justify-content: space-around;
	padding: 0 30rpx;
}
.navBtn {
	width: 210rpx;
	height: 70rpx;
	line-height: 70rpx;
	color: #007aff;
	background-color: white;
	border: 1rpx solid #007aff;
	display: flex;
	justify-content: center;
	align-items: center;
	font-size: 34rpx;
	letter-spacing: 5rpx;
	border-radius: 10rpx;
}
.f_text {
	text-align: center;
	font-size: 24rpx;
	height: 60rpx;
	line-height: 60rpx;
	color: #007aff;
	margin-top: 40rpx;
}
</style>
