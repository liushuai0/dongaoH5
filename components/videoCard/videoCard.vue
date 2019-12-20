<template>
	<view class="Container">
		
	</view>
</template>

<script>
export default {
	props: {
		videoList: {}
	},
	data() {
		return {
			title: 'Hello',
			currentPlay: null,
			
		};
	},
	computed: {},
	onLoad() {},
	onReady: function(res) {
		// this.videoContext = uni.createVideoContext('myVideo');
	},
	methods: {
		playvideo(e) {
			var index = e.currentTarget.dataset.index;
			console.log(index);
			this.currentPlay = index;
		},
		playend(e) {
			this.currentPlay = null;
		}
	},

};
</script>

<style>
	.coverImg{
	width: 335rpx;
	height: 200rpx;
	}
.none1 {
	display: none !important;
}
.qishi{
	width:750rpx;
	height:2000rpx;
}
.Container {
	display: flex;
	flex-direction: row;
		flex-wrap: wrap;
}
.videoCont {
	display: flex;
	flex-direction: column;
	position: relative;
	width: 335rpx;
	height: 250rpx;
	margin-bottom: 100rpx;
	margin: 20rpx;
}
.coverCont {
	width: 335rpx;
	height: 250rpx;
	display: flex;
	align-items: center;
	justify-content: center;
	background-color: #a1a1a1;
	border-radius: 5rpx;
}

.playicon {
	width: 60rpx;
	height: 60rpx;
	position: absolute;
}
.video1 {
	width: 335rpx;
	height: 250rpx;
		border-radius: 5rpx;
}
</style>
